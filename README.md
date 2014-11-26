pails-plugins
=============

A directory of and documentation for the pails plugin architecture

The plugin structure
--------------------

Each plugin has the following characteristics:
 * An "index" file that `require`s anything that is necessary for the library to operate.
   While this *can* include every file in your plugin, you may (depending on the number
   and use case of the files) want to instead provide an autoload function. You SHOULD NOT
   explicitly include any controllers or views you provide (in your plugin's `controllers`
   and `views` directory, respectively). You SHOULD explicitly include any models you
   provide (this is arguably an issue and likely to be fixed via an autoloader at some
   point in the future, so as to be backward-compatible). This file (and the files it
   includes) should only contain declarative code (class definitions and functions are
   OK). Any initialization or configuration code should be included in your
   `plugin_name_config` function.
 * A `plugin_name_config` function included somewhere along the line that is used to do
   any initialization or configuration for your module. This is 100% optional, as some
   plugins don't need any additional configuration. This function takes one argument,
   `$app`, which represents the current instance of the `\Pails\Application` that is
   running. See the API documentation or the source code for more details.
 * A `.pails` file in the root of the plugin. This is a JSON file with the following
   structure (keep in mind this is parsed by `json_decode`, so it must follow the original
   JSON spec to the letter -- double-quotes around all strings and keys, no line breaks in
   strings, etc.):

       {
         "name": "the_name_of_your_plugin",
         "index": "index.php",
         "deps": ["dependency1", "dependency2"]
       }

Notes on the .pails file
------------------------

1. The `name` of your plugin should be all lowercase, with spaces replaced by underscores.
   This is used to construct the name of your `config` function, so only use characters
   that are valid in this context.
2. The `index` file is traditionally named `index.php`, so you can probably use this line
   verbatim. If, for some reason, you already have an `index.php` that can't be renamed,
   the names `config.php` or `init.php` are probably good choices.
3. `deps` is a JSON array of strings, each string corresponding to the name of other
   plugins. Choose dependencies carefully. List only plugins that yours can't exist
   without. If your plugin can still exist without a plugin, but with reduced functionality,
   do so and do not list the plugin as a dependency. Shoot for empty arrays (or very short
   arrays) whenever possible.
