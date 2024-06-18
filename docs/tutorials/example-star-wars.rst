Example: StarWars import
========================================

.. note::
   | The code involved in this example is part of the test suite of **Yokai Batch**.
   | You can find the original code in the source repository:
     `Data <https://github.com/yokai-php/batch-src/tree/0.x/tests/symfony/data/star-wars>`__,
     `Entities <https://github.com/yokai-php/batch-src/tree/0.x/tests/symfony/src/Entity/StarWars>`__,
     `Jobs <https://github.com/yokai-php/batch-src/tree/0.x/tests/symfony/src/Job/StarWars>`__,
     `Tests <https://github.com/yokai-php/batch-src/blob/0.x/tests/symfony/tests/StarWarsJobSet.php>`__


What are we trying to do?
----------------------------------------

| We have been provided with `data from the StarWars universe <https://www.kaggle.com/jsphyg/star-wars>`__.
| Our job is to import these data in a relational database.

We will import the following data:

* Characters
* Species
* Planets

Designing the entities
----------------------------------------

.. code-block:: php

    <?php

    namespace App\Entity;

    use Doctrine\ORM\Mapping as ORM;
    use Symfony\Bridge\Doctrine\Validator\Constraints\UniqueEntity;
    use Symfony\Component\Validator\Constraints as Assert;

    #[ORM\Entity]
    #[ORM\Table(name: 'character')]
    #[UniqueEntity('name')]
    class Character
    {
        #[ORM\Column(type: 'integer')]
        #[ORM\Id]
        #[ORM\GeneratedValue(strategy: 'AUTO')]
        public int $id;

        #[ORM\Column(type: 'string', unique: true)]
        #[Assert\NotNull]
        public ?string $name;

        #[ORM\Column(type: 'integer', nullable: true)]
        public ?int $birthYear;

        #[ORM\Column(type: 'string')]
        #[Assert\NotNull]
        public ?string $gender;

        #[ORM\ManyToOne(targetEntity: Planet::class)]
        public ?Planet $homeWorld;

        #[ORM\ManyToOne(targetEntity: Specie::class)]
        public ?Specie $specie;
    }

.. code-block:: php

    <?php

    namespace App\Entity;

    use Doctrine\ORM\Mapping as ORM;
    use Symfony\Bridge\Doctrine\Validator\Constraints\UniqueEntity;
    use Symfony\Component\Validator\Constraints as Assert;

    #[ORM\Entity]
    #[ORM\Table(name: 'planet')]
    #[UniqueEntity('name')]
    class Planet
    {
        #[ORM\Column(type: 'integer')]
        #[ORM\Id]
        #[ORM\GeneratedValue(strategy: 'AUTO')]
        public int $id;

        #[ORM\Column(type: 'string', unique: true)]
        #[Assert\NotNull]
        public ?string $name;

        #[ORM\Column(type: 'integer', nullable: true)]
        public ?int $rotationPeriod;

        #[ORM\Column(type: 'integer', nullable: true)]
        public ?int $orbitalPeriod;

        #[ORM\Column(type: 'integer', nullable: true)]
        public ?int $population;

        #[ORM\Column(type: 'json')]
        #[Assert\NotNull]
        public array $terrain;
    }

.. code-block:: php

    <?php

    namespace App\Entity;

    use Doctrine\ORM\Mapping as ORM;
    use Symfony\Bridge\Doctrine\Validator\Constraints\UniqueEntity;
    use Symfony\Component\Validator\Constraints as Assert;

    #[ORM\Entity]
    #[ORM\Table(name: 'specie')]
    #[UniqueEntity('name')]
    class Specie
    {
        #[ORM\Column(type: 'integer')]
        #[ORM\Id]
        #[ORM\GeneratedValue(strategy: 'AUTO')]
        public int $id;

        #[ORM\Column(type: 'string', unique: true)]
        #[Assert\NotNull]
        public ?string $name;

        #[ORM\Column(type: 'string', nullable: true)]
        public ?string $classification;

        #[ORM\Column(type: 'string', nullable: true)]
        public ?string $language;

        #[ORM\ManyToOne(targetEntity: Planet::class)]
        public Planet $homeWorld;
    }

Writing the import
----------------------------------------

Install the packages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: console

    composer require yokai/batch
    composer require yokai/batch-openspout
    composer require yokai/batch-symfony-validator
    composer require yokai/batch-doctrine-persistence

An import for each entity
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    namespace App\Job\Import;

    use App\Entity\Planet;
    use Doctrine\Persistence\ManagerRegistry;
    use Symfony\Component\Validator\Validator\ValidatorInterface;
    use Yokai\Batch\Storage\JobExecutionStorageInterface;

    final class ImportStarWarsPlanetJob extends AbstractImportStartWarsEntityJob
    {
        public static function getJobName(): string
        {
            return 'star-wars.import:planet';
        }

        public function __construct(
            ValidatorInterface $validator,
            ManagerRegistry $doctrine,
            JobExecutionStorageInterface $executionStorage,
        ) {
            parent::__construct(
                __DIR__ . '/path/to/star-wars/planets.csv',
                function (array $item) {
                    $entity = new Planet();
                    $entity->name = $item['name'];
                    $entity->rotationPeriod = $item['rotation_period'] ? (int)$item['rotation_period'] : null;
                    $entity->orbitalPeriod = $item['orbital_period'] ? (int)$item['orbital_period'] : null;
                    $entity->population = $item['population'] ? (int)$item['population'] : null;
                    $entity->terrain = \array_filter(
                        \array_map('trim', \explode(',', (string)$item['terrain']))
                    );

                    return $entity;
                },
                $validator,
                $doctrine,
                $executionStorage,
            );
        }
    }

.. code-block:: php

    <?php

    namespace App\Job\Import;

    use App\Entity\Specie;
    use App\Entity\Planet;
    use Doctrine\Persistence\ManagerRegistry;
    use Symfony\Component\Validator\Validator\ValidatorInterface;
    use Yokai\Batch\Storage\JobExecutionStorageInterface;

    final class ImportStarWarsSpecieJob extends AbstractImportStartWarsEntityJob
    {
        public static function getJobName(): string
        {
            return 'star-wars.import:specie';
        }

        public function __construct(
            ValidatorInterface $validator,
            ManagerRegistry $doctrine,
            JobExecutionStorageInterface $executionStorage,
        ) {
            parent::__construct(
                __DIR__ . '/path/to/star-wars/species.csv',
                function (array $item) use ($doctrine) {
                    $entity = new Specie();
                    $entity->name = $item['name'];
                    $entity->classification = $item['classification'];
                    $entity->language = $item['language'];
                    if ($item['homeworld']) {
                        $entity->homeWorld = $doctrine->getRepository(Planet::class)
                            ->findOneBy(['name' => $item['homeworld']]);
                    }

                    return $entity;
                },
                $validator,
                $doctrine,
                $executionStorage,
            );
        }
    }

.. code-block:: php

    <?php

    namespace App\Job\Import;

    use App\Entity\Character;
    use App\Entity\Planet;
    use App\Entity\Specie;
    use Doctrine\Persistence\ManagerRegistry;
    use Symfony\Component\Validator\Validator\ValidatorInterface;
    use Yokai\Batch\Storage\JobExecutionStorageInterface;

    final class ImportStarWarsCharacterJob extends AbstractImportStartWarsEntityJob
    {
        public static function getJobName(): string
        {
            return 'star-wars.import:character';
        }

        public function __construct(
            ValidatorInterface $validator,
            ManagerRegistry $doctrine,
            JobExecutionStorageInterface $executionStorage,
        ) {
            parent::__construct(
                __DIR__ . '/path/to/star-wars/species.csv',
                function (array $item) use ($doctrine) {
                    $entity = new Character();
                    $entity->name = $item['name'];
                    $entity->birthYear = $item['birth_year'] ? (int)$item['birth_year'] : null;
                    $entity->gender = $item['gender'] ?? 'unknown';
                    $entity->homeWorld = $doctrine->getRepository(Planet::class)
                        ->findOneBy(['name' => $item['homeworld']]);
                    $entity->specie = $doctrine->getRepository(Specie::class)
                        ->findOneBy(['name' => $item['species']]);

                    return $entity;
                },
                $validator,
                $doctrine,
                $executionStorage,
            );
        }
    }

Factorise common logic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All three imports behavior the same way:

* read data from a CSV file
* convert data to an entity
* ensure entity is valid
* save entity to the database

| The thing is, most of the time, in your application, you will have similar jobs.
| **Yokai Batch** offers many reusable components, but you should also try to organise your code around.

| We chose the easiest way here: introducing an abstract class for all our jobs.
| We could have been creating a ``JobFactory``, but it's matter of taste.

.. code-block:: php

    <?php

    namespace App\Job\Import;

    use Closure;
    use Doctrine\Persistence\ManagerRegistry;
    use Symfony\Component\Validator\Validator\ValidatorInterface;
    use Yokai\Batch\Bridge\Doctrine\Persistence\ObjectWriter;
    use Yokai\Batch\Bridge\OpenSpout\Reader\FlatFileReader;
    use Yokai\Batch\Bridge\OpenSpout\Reader\HeaderStrategy;
    use Yokai\Batch\Bridge\Symfony\Framework\JobWithStaticNameInterface;
    use Yokai\Batch\Bridge\Symfony\Validator\SkipInvalidItemProcessor;
    use Yokai\Batch\Job\AbstractDecoratedJob;
    use Yokai\Batch\Job\Item\ItemJob;
    use Yokai\Batch\Job\Item\Processor\ArrayMapProcessor;
    use Yokai\Batch\Job\Item\Processor\CallbackProcessor;
    use Yokai\Batch\Job\Item\Processor\ChainProcessor;
    use Yokai\Batch\Job\Parameters\StaticValueParameterAccessor;
    use Yokai\Batch\Storage\JobExecutionStorageInterface;

    abstract class AbstractImportStartWarsEntityJob extends AbstractDecoratedJob implements JobWithStaticNameInterface
    {
        public function __construct(
            string $file,
            Closure $process,
            ValidatorInterface $validator,
            ManagerRegistry $doctrine,
            JobExecutionStorageInterface $executionStorage,
        ) {
            parent::__construct(
                new ItemJob(
                    50, // could be much higher, but you usually have to play around that value
                    new FlatFileReader(
                        new StaticValueParameterAccessor($file),
                        null,
                        null,
                        HeaderStrategy::combine(),
                    ),
                    new ChainProcessor([
                        new ArrayMapProcessor(
                            fn(string $value) => $value === 'NA' ? null : $value,
                        ),
                        new CallbackProcessor($process),
                        new SkipInvalidItemProcessor($validator),
                    ]),
                    new ObjectWriter($doctrine),
                    $executionStorage,
                ),
            );
        }
    }

A job for the whole import
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    namespace App\Job\Import;

    use Yokai\Batch\Job\AbstractDecoratedJob;
    use Yokai\Batch\Job\JobExecutor;
    use Yokai\Batch\Job\JobWithChildJobs;
    use Yokai\Batch\Storage\JobExecutionStorageInterface;

    final class ImportStarWarsJob extends AbstractDecoratedJob
    {
        public function __construct(JobExecutionStorageInterface $executionStorage, JobExecutor $jobExecutor)
        {
            parent::__construct(
                new JobWithChildJobs($executionStorage, $jobExecutor, [
                    ImportStarWarsPlanetJob::getJobName(),
                    ImportStarWarsSpecieJob::getJobName(),
                    ImportStarWarsCharacterJob::getJobName(),
                ]),
            );
        }
    }

Running the import
----------------------------------------

.. code-block:: php

    <?php

    namespace App\Command;

    use App\Job\Import\ImportStarWarsJob;
    use Symfony\Component\Console\Attribute\AsCommand;
    use Symfony\Component\Console\Command\Command;
    use Symfony\Component\Console\Input\InputInterface;
    use Symfony\Component\Console\Output\OutputInterface;
    use Yokai\Batch\Launcher\JobLauncherInterface;

    #[AsCommand(name: 'app:import')]
    final class ImportCommand extends Command
    {
        public function __construct(
            private readonly JobLauncherInterface $jobLauncher,
        ) {
            parent::__construct();
        }

        protected function execute(InputInterface $input, OutputInterface $output): int
        {
            $this->jobLauncher->launch(ImportStarWarsJob::getJobName());

            return self::SUCCESS;
        }
    }

.. seealso::
   | :doc:`What is an item job? </core-concepts/item-job>`
