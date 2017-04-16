CMS Extension
========================
This module provides a web interface for content management system and includes following features:

- Allows CRUD operations for pages
- [Support Markdown Editor](https://github.com/yii2mod/yii2-markdown)
- [Support Comments Management System](https://github.com/yii2mod/yii2-comments)
- Integrated with [yii2mod/base](https://github.com/yii2mod/base)

[![Latest Stable Version](https://poser.pugx.org/yii2mod/yii2-cms/v/stable)](https://packagist.org/packages/yii2mod/yii2-cms) [![Total Downloads](https://poser.pugx.org/yii2mod/yii2-cms/downloads)](https://packagist.org/packages/yii2mod/yii2-cms) [![License](https://poser.pugx.org/yii2mod/yii2-cms/license)](https://packagist.org/packages/yii2mod/yii2-cms)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/yii2mod/yii2-cms/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/yii2mod/yii2-cms/?branch=master) [![Build Status](https://travis-ci.org/yii2mod/yii2-cms.svg?branch=master)](https://travis-ci.org/yii2mod/yii2-cms)


Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```bash
php composer.phar require --prefer-dist yii2mod/yii2-cms "*"
```

or add

```
"yii2mod/yii2-cms": "*"
```

to the require section of your composer.json.


CONFIGURATION
------------

**Database Migrations**

Before usage this extension, we'll also need to prepare the database.

```bash
$ php yii migrate --migrationPath=@vendor/yii2mod/yii2-comments/migrations
$ php yii migrate --migrationPath=@vendor/yii2mod/yii2-cms/migrations
```

**Module Setup**

To access the module, you need to configure the modules array in your application configuration:

```php
'modules' => [
    'cms' => [
        'class' => 'yii2mod\cms\Module',
    ],
],
```
> **You can then access to management section through the following URL:**

> http://localhost/path/to/index.php?r=/cms/manage/index
  

2) Add new Rule class to the `urlManager` array in your application configuration by the following code:
 
```php
 'components' => [
     'urlManager' => [
         'rules' => [
             ['class' => 'yii2mod\cms\components\PageUrlRule'],
         ]
     ],
 ],
```

3) Add to SiteController (or configure via `$route` param in `urlManager`):
```php
public function actions()
{
    return [
        'page' => [
            'class' => 'yii2mod\cms\actions\PageAction',
        ]
    ];
}
```
> And now you can create your own pages via admin panel.

## Features:

1. Markdown Editor support:
```php
'modules' => [
    'cms' => [
        'class' => 'yii2mod\cms\Module',
        'enableMarkdown' => true,
        // List of options: https://github.com/NextStepWebs/simplemde-markdown-editor#configuration
        'markdownEditorOptions' => [
            'showIcons' => ['code', 'table'],
        ],
    ],
],
```

2. You can insert your own widget on the page by the following steps:

- Create the widget, for example:

```php
namespace app\widgets;

use yii\base\Widget;

class MyWidget extends Widget
{
   /**
    * @inheritdoc
    */
   public function run()
   {
       parent::run();

       echo 'Text from widget';
   }

   /**
    * This function used for render the widget
    *
    * @return string
    */
   public static function show()
   {
       return self::widget();
   }
}
```
    
- When you create the page via admin panel add the following code to the page content:
    
```
 [[\app\widgets\MyWidget:show]]
```

3. You can use parameters in your page content, for example: {siteName}, {homeUrl}. For parsing this parameters you can use the `baseTemplateParams` property:

```php
public function actions()
{
    return [
        'page' => [
            'class' => 'yii2mod\cms\actions\PageAction',
            'baseTemplateParams' => [
               'homeUrl' => 'your site home url',
               'siteName' => Yii::$app->name
            ]
        ],
    ];
}
```
