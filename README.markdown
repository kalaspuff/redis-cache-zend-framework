Redis cache backend for Zend Framework by Carl Oscar Aaro
=============
Requires the phpredis extension for PHP to enable PHP to communicate with the [Redis](http://redis.io/) key-value store.
Start with installing the phpredis PHP extension available at https://github.com/nicolasff/phpredis

Set up the Redis cache backend by invoking the following code somewhere in your project.

<pre>
$redisCache = Zend_Cache::factory(
    new Zend_Cache_Core(array(
        'lifetime' => 3600,
        'automatic_serialization' => true,
    )),
    new Extended_Cache_Backend_Redis(array(
        'servers' => array(
            array(
                'host' => '127.0.0.1',
                'port' => 6379,
                'dbindex' => 1,
                // 'persistent' => false, // not a persistent connection
                // 'auth' => true, // enable authentication
                // 'password' => 'mypwd000000', // password to authenticate on redis server
            ),
        ),
        // 'key_prefix' => 'my_app', // if desire to add a prefix to all cache keys
    ))
);
</pre>

Writing and reading from the cache works the same way as all other Zend Framework cache backends.

<pre>
$cacheKey = 'my_key';
$data = 'e48e13207341b6bffb7fb1622282247b';

/* Save data to cache */
$redisCache->save($data, $cacheKey, array('tag1', 'tag2'));

/* Load data from cache */
$data = $redisCache->load($cacheKey);

/* Clear all keys with tag 'tag1' */
$redisCache->clean(Zend_Cache::CLEANING_MODE_MATCHING_ANY_TAG, array('tag1'));

/* Clear all cache (flush cache) */
$redisCache->clean(Zend_Cache::CLEANING_MODE_ALL);
</pre>
