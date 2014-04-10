loid
====

Implements LightOpenID class support into Yii Framework

This extension based on [LightOpenID](http://gitorious.org/lightopenid "LightOpenID") class.
##Intro
An PHP 5 library for easy openid authentication. Works only as a consumer. more…

Features:

    * Easy to use. (you can code a functional client in less than ten lines of code)
    * Depends only on curl
    * Supports both OpenID 1.1 and 2.0
    * Supports Yadis discovery
    * Supports only stateless/dumb protocol
    * Works with PHP >= 5
    * Generates no errors with error_reporting(E_ALL | E_STRICT) 

##About extension
* This extension is directory independent, you can use it any desired directory without rewritting `import()` instructions.
* Extension is fully updatable. You can rewrite LightOpenID class with fresh version with any newer LightOpenID version without any corrections in extension code.


##Requirements

CURL.

##Installation

First you must install extension to your project:  
You can do it in 2 ways:

**Way 1:** in your project config.php in the components section, add the following:
~~~
[php]
'loid' => array(
               //alias to dir, where you unpacked extension
    'class' => 'application.extensions.lightopenid.loid',
),
~~~

**Way 2:** in your controller code:
~~~
[php]
Yii::app()->setComponents(array('loid'=>array('class'=>'application.extensions.lightopenid.loid')));
),
~~~

##Usage

Simple usage:
~~~
[php]
$loid = Yii::app()->loid->load();
if (!empty($_GET['openid_mode'])) {
    if ($_GET['openid_mode'] == 'cancel') {
        $err = Yii::t('core', 'Authorization cancelled');
    } else {
        try {
            echo $loid->validate() ? 'Logged in.' : 'Failed';
        } catch (Exception $e) {
            $err = Yii::t('core', $e->getMessage());
        }
    }
    if(!empty($err)) echo $err;
} else {
    $loid->identity = "http://my.openid.identifier"; //Setting identifier
    $loid->required = array('namePerson/friendly', 'contact/email'); //Try to get info from openid provider
    $loid->realm     = (!empty($_SERVER['HTTPS']) ? 'https' : 'http') . '://' . $_SERVER['HTTP_HOST']; 
    $loid->returnUrl = $loid->realm . $_SERVER['REQUEST_URI']; //getting return URL
    if (empty($err)) {
        try {
            $url = $loid->authUrl();
            $this->redirect($url);
        } catch (Exception $e) {
            $err = Yii::t('core', $e->getMessage());
        }
    }
}
~~~

##Configuration:
You can set loid configuration by 2 ways:  

**01.** After load configuration:
~~~
[php]
$loid = Yii::app()->loid->load();
$loid->identity = "http://my.openid.identifier"; //Setting identifier
$loid->required = array('namePerson/friendly', 'contact/email'); //Try to get info from openid provider
~~~
**02.** Onload configuration:
~~~
[php]
$config = array('identity'=>'http://my.openid.identifier','required'=>array('namePerson/friendly', 'contact/email'));
$loid = Yii::app()->loid->load($config);
~~~

##LightOpenid usage ReadMe:
~~~
[php]
/**
 * This class provides a simple interface for OpenID (1.1 and 2.0) authentication.
 * Supports Yadis discovery.
 * The authentication process is stateless/dumb.
 *
 * Usage:
 * Sign-on with OpenID is a two step process:
 * Step one is authentication with the provider:
 * <code>
 * $openid = new LightOpenID;
 * $openid->identity = 'ID supplied by user';
 * header('Location: ' . $openid->authUrl());
 * </code>
 * The provider then sends various parameters via GET, one of them is openid_mode.
 * Step two is verification:
 * <code>
 * if ($this->data['openid_mode']) {
 *     $openid = new LightOpenID;
 *     echo $openid->validate() ? 'Logged in.' : 'Failed';
 * }
 * </code>
 *
 * Optionally, you can set $returnUrl and $realm (or $trustRoot, which is an alias).
 * The default values for those are:
 * $openid->realm     = (!empty($_SERVER['HTTPS']) ? 'https' : 'http') . '://' . $_SERVER['HTTP_HOST'];
 * $openid->returnUrl = $openid->realm . $_SERVER['REQUEST_URI'];
 * If you don't know their meaning, refer to any openid tutorial, or specification. Or just guess.
 *
 * AX and SREG extensions are supported.
 * To use them, specify $openid->required and/or $openid->optional before calling $openid->authUrl().
 * These are arrays, with values being AX schema paths (the 'path' part of the URL).
 * For example:
 *   $openid->required = array('namePerson/friendly', 'contact/email');
 *   $openid->optional = array('namePerson/first');
 * If the server supports only SREG or OpenID 1.1, these are automaticaly
 * mapped to SREG names, so that user doesn't have to know anything about the server.
 *
 * To get the values, use $openid->getAttributes().
 *
 *
 * The library requires PHP >= 5.1.2 with curl or http/https stream wrappers enabled.
 * @author Mewp
 * @copyright Copyright (c) 2010, Mewp
 * @license http://www.opensource.org/licenses/mit-license.php MIT
 */
~~~

##i18n
Here are i18n pairs to `Yii::t()` instructions:

~~~
[php]
'No servers found!' => 'Не найден сервер авторизации. (No servers found!)',
'Invalid request.'=> 'Неверный запрос. (Invalid request.)',
'No identity supplied.'=>'Нет поддержки идентификатора. (No identity supplied.)',
'Endless redirection!'=>'Бесконечный редирект! (Endless redirection!)',
~~~


##Resources

 * [LightOpenID in Google code](http://code.google.com/p/lightopenid/)
 * [LightOpenID in Gitorious](http://gitorious.org/lightopenid)
