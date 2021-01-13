# Magento2-MM-CSP-fix
Patched file for MageMojo Google session collection when Content Security Policies (CSP) enabled or enforced.

Basically here we follow https://devdocs.magento.com/guides/v2.3/comp-mgr/patching/composer.html official M2 DevDocs guidelines.

1) Open your command line application and navigate to your project directory.
2) Add the cweagans/composer-patches plugin to the composer.json file.


composer require cweagans/composer-patches
3) Edit the composer.json file and add the following section to specify:


        "patches": {
        "magento/module-google-adwords": {
              "INFRA-123456: MageMojo CSP fix to whitelist googletagmanager.com domain": "https://github.com/magemojo/Magento2-MM-CSP-fix/commit/5faa8f1619e4f6b600ce4fedb9a54d7fe5a4debb.patch"
          }
      }
Text is added in the "extra": { section, with following content:

Module: "magento/module-google-adwords"   ← That’s the Magento 2 Core module we are patching

Title: We link this with internal STRAT or INFRA tasks.

URL to patch: "https://github.com/magemojo/Magento2-MM-CSP-fix/pull/2/commits/5faa8f1619e4f6b600ce4fedb9a54d7fe5a4debb " ← That’s link from above and our demo repository.

If a patch affects multiple modules, you must create multiple patch files targeting multiple modules.

4)  Apply the patch. Use the -v option only if you want to see debugging information.
composer -v install

5) Update the composer.lock file. The lock file tracks which patches have been applied to each  Composer package in an object.
composer update --lock

- Everything ready!!! Lucky each Magento 2 core module comes with integration/unit tests ready. For the end let’s run the PHP Unit test against the Core module we just patched with changes.

https://devdocs.magento.com/guides/v2.4/test/unit/unit_test_execution_cli.html

Edit dev/tests/unit/phpunit.xml.dist file and under <testsuite name="Magento_Unit_Tests_Other"> just keep following:

<directory>../../../vendor/magento/module-google-adwords/Test/Unit</directory>
Remove everything else since we are running test only against one specific Magento 2 core module.

Execute following:
./vendor/bin/phpunit -c dev/tests/unit/phpunit.xml.dist vendor/magento/module-google-adwords/Test/Unit 
