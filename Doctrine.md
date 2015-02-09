Doctrine
========

## Entity Manager

#### `$em = $this->getDoctrine()->getManager()`
Returns an entity manager

#### `$entity = new Entity()`
Instanciates a new Entity object

#### `$entity->setProperty($value)`
Sets some property of the entity

#### `$em->persist($entity)`
Tells the entity manager to manage a recently created entity

#### `$em->remove()`
Delete the entity from database (when flushed)

#### `$em->flush()`
Tells Doctrine to look at all managed entities and update database if needed


## Repository

#### `$repository = $this->getDoctrine()->getRepository('{Vendor}{Bundle}:{Entity}')`
Gets the repository for {Vendor}\{Bundle}\Entity\{Entity}


### Methods

#### `$entry = $repository->find($id)`
Finds an entry by PRIMARY KEY (`WHERE id = `)

#### `$entry = $repository->findAll()`
Finds all entries

#### `$entry = $repository->findOneByProperty($value)`
Finds an entry by specific property (`WHERE property = $value`)

#### `$entry = $repository->findByProperty($value)`
Finds all entries matching `WHERE property = $value`

#### `$entry = $repository->findOneBy(array('property1' => $value1, 'property2' => $value2))`
Finds an entry matching multiple critera (`WHERE ... AND ...`)

#### `$entry = $repository->findBy(array('property1' => $value1, 'property2' => $value2))`
Finds all entries matching multiple critera (`WHERE ... AND ...`)


### Custom methods

#### `$entry = $repository->findWithCustomMethod()`
findWithCustomMethod using DQL or QueryBuilder can be defined in `src/Vendor/Bundle/Entity/EntityRepository.php`


## Doctrine Query Language

#### Create a custom query with DQL

    $query = $em->createQuery(
        'SELECT p
        FROM {Vendor}{Bundle}:{Entity} e
        WHERE e.property > :value
        ORDER BY e.property DESC
    );
    
#### `$query->setParameter(':value', $value)`
Set prepared query parameter

#### `$query->setParameters(array(':value1' => $value1, ':value2' => $value2))`
Set multiple parameters for prepared query
    
#### `$entities = $query->getResults()`
Returns an array of result

#### `$entitiy = $query->getResult()`
Returns only one result (throws exceptions if result count = 0 or > 1)


## QueryBuilder

#### Create a custom query with the QueryBuilder

    $query = $repository->createQueryBuilder('e')
        ->where('e.property > :value')
        ->setParameter(':value', value)
        ->orderBy('e.property', 'DESC')
        ->getQuery();
        
    $entities = $query->getResult();

[http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/query-builder.html](Reference)



## Mapping


### Define field

    /**
     * @var string
     *
     * @ORM\Column(name="name", type="string", length=255)
     */
    private $name;
    
### Define relation

    /**
     * @ORM\ManyToOne(targetEntity="Publisher", inversedBy="articles")
     * @ORM\JoinColumn(name="publisher_id", referencedColumnName="publisher_id")
     */
    protected $publisher;
    
    /**
     * @ORM\OneToMany(targetEntity="Article", mappedBy="publisher")
     */
    protected $articles;

### Field options

* name: field name (default: property name)
* type: field type (default: string)
* length: field length (default: 255)
* unique: true/false (default: false)
* nullable: true/fase (default: false)
* options: key/value pairs for plateform-specific options
    * test


### Field types

* string: Type that maps a SQL VARCHAR to a PHP string.
* integer: Type that maps a SQL INT to a PHP integer.
* smallint: Type that maps a database SMALLINT to a PHP integer.
* bigint: Type that maps a database BIGINT to a PHP string.
* boolean: Type that maps a SQL boolean or equivalent (TINYINT) to a PHP boolean.
* decimal: Type that maps a SQL DECIMAL to a PHP string.
* date: Type that maps a SQL DATETIME to a PHP DateTime object.
* time: Type that maps a SQL TIME to a PHP DateTime object.
* datetime: Type that maps a SQL DATETIME/TIMESTAMP to a PHP DateTime object.
* datetimetz: Type that maps a SQL DATETIME/TIMESTAMP to a PHP DateTime object with timezone.
* text: Type that maps a SQL CLOB to a PHP string.
* object: Type that maps a SQL CLOB to a PHP object using serialize() and unserialize()
* array: Type that maps a SQL CLOB to a PHP array using serialize() and unserialize()
* simple_array: Type that maps a SQL CLOB to a PHP array using implode() and explode(), with a comma as delimiter. IMPORTANT Only use this type if you are sure that your values cannot contain a ”,”.
* json_array: Type that maps a SQL CLOB to a PHP array using json_encode() and json_decode()
* float: Type that maps a SQL Float (Double Precision) to a PHP double. IMPORTANT: Works only with locale settings that use decimal points as separator.
* guid: Type that maps a database GUID/UUID to a PHP string. Defaults to varchar but uses a specific type if the platform supports it.
* blob: Type that maps a SQL BLOB to a PHP resource stream


### Console commands

#### `doctrine:mapping:import`
Creates mapping information from existing database

#### `doctrine:mapping:info`
Shows information about mapped entities

#### `doctrine:mapping:import`
Creates mapping information from existing database

### `doctrine:migrations:status`
Shows doctrine migrations status

### `doctrine:migrations:generate`
Generates an empty migration class

### `doctrine:migrations:diff`
Automatically generates the migration class base on differences between entity model and database

### `doctrine:migrations:migrate`
Execute all migrations classes
