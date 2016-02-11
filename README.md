WPML to Polylang
================
This guide is intended for sites that have an active [WPML][1] installation and want to migrate to [Polylang][2].

[![GitHub license](https://img.shields.io/badge/license-CC0%201.0-orange.svg?style=flat-square)](http://creativecommons.org/publicdomain/zero/1.0/)

Some considerations
-------------------
Carefully assess whether this move is in any way suitable for your current site. Many of the features WPML offers out of the box are not available for Polylang and will require you to find add-ons to restore some of these features. The [WordPress plugin repository][8] is a good place to start. Your next best bet is [GitHub][9].

Do your homework and make sure all the features you currently have are available for Polylang in one form or another. The most common issue is localizing permalinks for Custom Post Types, but luckily [there is an add-on][5] for that.

Before continuing, make sure you have [backed up your WordPress database][3] and have a copy of your entire site in case you need to restore it after these changes.

String Translation
------------------
If you've been using WPML's [String Translation][10] to translate your theme, you can use a [similar solution][17] from Polylang or do it the [WordPress way][11].

Opting for the Polylang solution will require you to register strings in your theme with its own `pll_` [functions][12], but if you want to create a future-proof theme it is recommended that you familiarize yourself with the [Internationalization][13] part of the new [Theme Developer Handbook][27] and do the localization yourself via [language files][28].

In any case, you will need to [export your current WPML string translations][10] to an external `.po` file. All traces of WPML will be removed by the end of this tutorial. _Export them now_.

### Exporting strings

You can either export all contexts at once, or go through the relevant ones and export them to separate `.po` files. These `.po` files can be used as reference when building your theme's `.po` files from scratch using tools like [Poedit][14], Grunt or any other localization solution. If you're going for Polylang's Strings Translation, you will need to copy paste them by hand.

Note that basic things such as the site title, description and widget titles will be automatically added to Polylang's own equivalent.

If you're unsure about which contexts to export, you might as well export all of them for safe keeping. Remember to export each language individually from the box at the bottom of WPML's _String Translation_ page.

Getting started
---------------
At the time of writing, Polylang and WPML **can not** be active at the same time or your site _will crash_. If you run into this issue, rename the Polylang folder to something else, refresh your browser and rename it back to what it was when you have restored functionality to your site.

Again, make sure you have backed up your [WordPress database][3] and have a copy of all your files before continuing.

### Importing WPML settings to Polylang
Go through the following steps one by one:

1. Deactivate **WPML** and all related add-ons
2. Download and activate [Polylang][2]
3. Download and activate [WPML to Polylang][15]
4. Go to `Tools > WPML importer`
5. If everything is green, click on `Import`
6. Deactivate and delete the **WPML to Polylang** plugin

### Setting up Polylang
1. Go to `Settings > Languages` and verify that all previously configured languages are present ([documentation][16])
2. Check the `Strings Translation` tab. It should contain the basic translation of your site and widget titles ([documentation][17])
3. Switch to the `Settings` tab and go through each setting ([documentation][18])
4. When you're done, click `Save Changes`

### Removing WPML
**Warning**: After this there is no turning back. The following steps will **permanently remove** all WPML tables from your database and get rid of WPML from your current site.

1. Deactivate **Polylang**
2. Activate **WPML**
3. Go to `WPML > Support` and click on the `troubleshooting` link at the bottom of the page
4. Scroll down to the `Reset` box
5. Tick `[x] I am about to reset all language data`
6. Click and confirm `Reset all language data and deactivate WPML`
7. Deactivate and delete and all **WPML** related plugins
8. Reactivate **Polylang** and the add-ons you found to replace those you just deleted

Theme conversion
----------------
You will most likely have to do some work to make your existing theme compatible with Polylang. The [documentation for developers][6] is a good place to start. Search the web for any existing add-ons or solutions that might already solve the problems you are facing. This might end up saving you a lot of time.

### Advanced Custom Fields
If you have used WPML in conjunction with [Advanced Custom Fields][19] to retrieve entries in the options page by appending the current language code to a custom field, you can use Polylang's `pll_current_language()` instead of `ICL_LANGUAGE_CODE` to get the current langugage.

### Categories and post tags
Go through each of your categories and post tags to verify the translations were imported correctly. Save them again for good measure and refer to the [documentation][20] if you're not sure how to proceed.

### Custom Post Types
If you had previously translated Custom Post Types and their permalinks with WPML, you can use Klicher's [Translate URL Rewrite Slugs][5] to do the same. It might not be as pretty, but you can always contribute on GitHub to make it better.

### Language Switcher
The markup between the two plugins is a little different, so it is unlikely your new [Language Switcher][21] will work out of the box. Again, Polylang's [developer pages][6] are your friend. You might also want to take a look at the rendered source code to find out which CSS classes you will need to use.

### Navigation Menus
Your navigation menus might also need a small touch up depending on how you have registered them in the past. Refer to the [documentation][22].

### Widgets
The widget titles should now be translatable through Polylang's Strings translation which you will find under `Settings > Languages`.

### WPML related functions and hooks
Scan through your theme and look for any `icl` related functions and definitions. Polylang _does_ support some of these, but it also has its own equivalents which you can find on the [Compatibility with the WPML API][23] page. It is strongly recommended you make the extra effort to convert them. There is no telling how well these polyfills will work in the future.

Also look for anything starting with `wpml` such as [apply_filters() hooks][24].

Finally, remove any [custom capabilities][25] you may have used to give users access to certain parts of the site. You will most likely need a plugin to manage these capabilities in the future.

Users and workflow
------------------
Review your user base and figure out what to do with accounts (translators) that were solely devoted to [Translation Management][26]. If you were not using this feature by WPML, you should still go through the different workflows to ensure that the correct users are still able to edit and submit translations.

Missing features
----------------
If you end up developing missing features for Polylang to fill up voids left by WPML or its add-ons, think how you could make it reusable and share your knowledge with the community – preferably in the form of a plugin. Chances are someone else has run into the same issue and is looking for the same answers.

[Polylang][2] is still relatively new and needs all the support it can get!

[1]: https://wpml.org "Download WPML"
[2]: https://wordpress.org/plugins/polylang/ "Download Polylang"
[3]: https://codex.wordpress.org/WordPress_Backups "How to backup the WordPress database"
[4]: https://polylang.wordpress.com/documentation/documentation-for-developers/compatibility-with-the-wpml-api/ "Compatibility with the WPML API"
[5]: https://github.com/KLicheR/wp-polylang-translate-rewrite-slugs "Polylang - Translate URL Rewrite Slugs"
[6]: https://polylang.wordpress.com/documentation/documentation-for-developers/ "Polylang - Documentation for developers"
[7]: https://polylang.wordpress.com/documentation/documentation-for-developers/functions-reference/#pll_the_languages "Polylang - Language switcher"
[8]: https://wordpress.org/plugins/search.php?q=polylang "Search for Polylang add-ons in the WordPress plugin repository"
[9]: https://github.com/search?q=polylang "Search for Polylang add-ons on GitHub"
[10]: https://wpml.org/documentation/getting-started-guide/string-translation/ "WPML - String Translation"
[11]: https://developer.wordpress.org/plugins/internationalization/localization/ "WordPress - Localization"
[12]: https://polylang.wordpress.com/documentation/documentation-for-developers/functions-reference/#pll_register_string "Polylang - Register strings for Strings Translation"
[13]: https://developer.wordpress.org/themes/functionality/internationalization/ "WordPress - Internationalization"
[14]: http://poedit.net/ "Download Poedit"
[15]: https://wordpress.org/plugins/wpml-to-polylang/ "Download the WPML to Polylang plugin"
[16]: https://polylang.wordpress.com/documentation/setting-up-a-wordpress-multilingual-site-with-polylang/creating-the-languages/ "Polylang - Create new languages"
[17]: https://polylang.wordpress.com/documentation/setting-up-a-wordpress-multilingual-site-with-polylang/strings-translation/ "Polylang - Strings Translation"
[18]: https://polylang.wordpress.com/documentation/setting-up-a-wordpress-multilingual-site-with-polylang/settings/ "Polylang - Settings page"
[19]: http://www.advancedcustomfields.com/ "Download Advanced Custom Fields"
[20]: https://polylang.wordpress.com/documentation/setting-up-a-wordpress-multilingual-site-with-polylang/translating-categories-or-post-tags/ "Polylang - Translating categories and post tags"
[21]: https://polylang.wordpress.com/documentation/documentation-for-developers/functions-reference/#pll_the_languages "Polylang - Language Switcher"
[22]: https://polylang.wordpress.com/documentation/setting-up-a-wordpress-multilingual-site-with-polylang/navigations-menus/ "Polylang - Navigation menus"
[23]: https://polylang.wordpress.com/documentation/documentation-for-developers/compatibility-with-the-wpml-api/ "Polylang - WPML API compatibility"
[24]: https://wpml.org/documentation/support/wpml-coding-api/wpml-hooks-reference/ "WPML - Hooks reference"
[25]: https://wpml.org/documentation/support/wpml-admin-capabilities/ "WPML - Custom user capabilities"
[26]: https://wpml.org/documentation/translating-your-contents/using-the-translation-editor/ "WPML - Translation Management"
[27]: https://developer.wordpress.org/themes/getting-started/ "WordPress - Theme Developer Handbook"
[28]: https://developer.wordpress.org/themes/functionality/localization/ "WordPress - Localization"
