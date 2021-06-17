<div  align="center">
	<a  href="https://www.storyblok.com?utm_source=github.com&utm_medium=referral&utm_campaign=php-client"  align="center">
		<img  src="https://a.storyblok.com/f/51376/x/19b4879a86/logo.svg"  width="200"  alt="Storyblok Logo">
	</a>
	<h1  align="center">Storyblok PHP Client</h1>
	<p  align="center">This is the official <a href="https://www.storyblok.com?utm_source=github.com&utm_medium=referral&utm_campaign=php-client">Storyblok</a> PHP client to easy access the content deliver api and management api.</p>
</div>

<p align="center">
  <a href="">
   <img src="https://img.shields.io/discord/700316478792138842?label=Join%20Our%20Discord%20Community&style=flat-square&logo=discord&color=09b3af">
   </a>
<a href="https://twitter.com/intent/follow?screen_name=storyblok">
    <img src="https://img.shields.io/twitter/follow/storyblok?style=social" alt="Follow @Storyblok" />
  </a>
</p>

## 🚀 Getting Started

The content delivery client checks the get parameters _storyblok to get the draft version of a specific story and _storyblok_published to clear the cache.

### Install

```bash
composer require storyblok/php-client dev-master
```

### Initialization

Initialize the content delivery client with your space draft token for read operations

```php
$client = new \Storyblok\Client('your-storyblok-draft-token');
```

Initialize the management client with your OAuth token from the my account section for read and write operations

```php
$managementClient = new \Storyblok\ManagementClient('your-storyblok-oauth-token');
```

### Usage of the management client


GET calls

```php
$spaceId = 'YOUR_SPACE_ID';
$managementClient->get('spaces/' . $spaceId . '/stories')->getBody();
```

POST calls

~~~php
$spaceId = 'YOUR_SPACE_ID';
$managementClient->post('spaces/' . $spaceId . '/stories', ['space' => ['name' => 'Manage']])->getBody();
~~~

PUT calls

~~~php
$spaceId = 'YOUR_SPACE_ID';
$storyId = 'YOUR_STORY_ID';
$managementClient->put('spaces/' . $spaceId . '/stories/' . $storyId, ['space' => ['name' => 'Manage']])->getBody();
~~~

DELETE calls

~~~php
$spaceId = 'YOUR_SPACE_ID';
$storyId = 'YOUR_STORY_ID';
$managementClient->delete('spaces/' . $spaceId . '/stories/' . $storyId)->getCode();
~~~



### Usage of the content delivery client

#### Load a Story

```php
// Require composer autoload
require 'vendor/autoload.php';

// Initialize
$client = new \Storyblok\Client('your-storyblok-private-token');

// Optionally set a cache
$client->setCache('filesytem', array('path' => 'cache'));

// Get the story searching for the slug
$client->getStoryBySlug('home');
$data = $client->getBody();

// Get the story searching for the uuid
$client->getStoryByUuid('0c092d14-5cd4-477e-922c-c7f8e330aaea');
$data = $client->getBody();
```

#### Load a list of Stories

```php
// Require composer autoload
require 'vendor/autoload.php';

// Initialize
$client = new \Storyblok\Client('your-storyblok-private-token');

// Optionally set a cache
$client->setCache('filesytem', array('path' => 'cache'));

// Get all Stories that start with news
$client->getStories(
    array(
        'starts_with' => 'news'
    )
);
$data = $client->getStoryContent();
```

#### Load a list of datasource entries

```php
// Require composer autoload
require 'vendor/autoload.php';

// Initialize
$client = new \Storyblok\Client('your-storyblok-private-token');

// Optionally set a cache
$client->setCache('filesytem', array('path' => 'cache'));

// Get all Stories that start with news
$client->getDatasourceEntries('categories');

// will return the whole response
$data = $client->getBody();

// will return as ['name']['value'] Array for easy access
$nameValueArray = $client->getAsNameValueArray();

```

#### Load a list of tags

```php
// Require composer autoload
require 'vendor/autoload.php';

// Initialize
$client = new \Storyblok\Client('your-storyblok-private-token');

// Optionally set a cache
$client->setCache('filesytem', array('path' => 'cache'));

// Get all Tags
$client->getTags();

// will return the whole response
$data = $client->getBody();

// will return as ['tagName1', 'tagName2'] Array for easy access
$stringArray = $client->getAsStringArray();

```

#### Load a list of tags and get the Respones Headers

```php
// Require composer autoload
require 'vendor/autoload.php';

// Initialize
$client = new \Storyblok\Client('your-storyblok-private-token');

// Optionally set a cache
$client->setCache('filesytem', array('path' => 'cache'));

// Get all Tags
$client->getTags();

// Let's you acces the Headers
var_dump($client->getHeaders());

```

### Clearing the cache (Optionally if using setCache)

In order to flush the cache when the user clicks publish, you need to listen to the published event in javascript or define a webhook in the space settings that clears the cache on your server.

```html
<script type="text/javascript" src="//app.storyblok.com/f/storyblok-latest.js"></script>
<script type="text/javascript">
    storyblok.init()

    storyblok.on('published', function() {
        $.ajax({
            url: '/clear.php'
        })
    })
</script>
```

In clear.php:
```php
$client = new \Storyblok\Client('your-storyblok-private-token');
$client->setCache('filesytem', array('path' => 'cache'));

// Flush the whole cache when a story has been published
$client->flushCache();

// Or empty the cache for one specific item only
$client->deleteCacheBySlug('home');
```


### Generate a navigation tree

```php
$tree = $client->editMode()->getLinks()->getAsTree();

echo '<ul>';
foreach ($tree as $item) {
    echo '<li>' . $item['item']['name'];

    if (!empty($item['children'])) {
        echo '<ul>';
        foreach ($item['children'] as $item2) {
            echo '<li>' . $item2['item']['name'] . '</li>';
        }
        echo '</ul>';
    }

    echo '</li>';
}
echo '</ul>';
```

#### Nginx SSI - Server Side Includes

Use the following script if you have Nginx SSI enabled and experience issues with printing the _editable html comments directly to manually parse the Storyblok HTML editable comments: https://gist.github.com/DominikAngerer/ca61d41bae3afcc646cfee286579ad36

## ℹ️ Support

* Bugs or Feature Requests? [Submit an issue](https://github.com/storyblok/wordpress-importer/issues/new);

* Do you have questions about Storyblok or you need help? [Join our Discord Community](https://discord.gg/jKrbAMz).

## 🤝  Contributing

Please see our [contributing guidelines](https://github.com/storyblok/.github/blob/master/contributing.md) and our [code of conduct](https://www.storyblok.com/trust-center#code-of-conduct).

## 📄 License

This repository is published under the [MIT](./LICENSE) license.

## 🔗 Related Resources

* **[Storyblok PHP Richtext Renderer](https://github.com/storyblok/storyblok-php-richtext-renderer)** : This package allows you to get an HTML string from the richtext field of Storyblok;

* **[Storyblok Laravel Tutorial](https://www.storyblok.com/tp/add-a-headless-cms-to-laravel-in-5-minutes?utm_source=github.com&utm_medium=referral&utm_campaign=php-client)** : Add a Headless CMS to Laravel in 5 minutes.
