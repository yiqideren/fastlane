# fastlane plugins troubleshooting

If you're having trouble calling a plugin action, here is a simple guide on how to resolve the issue:

### Make sure fastlane is up to date

Run `fastlane -v` and `bundle exec fastlane -v` and make sure it's at least version 1.93.0.

### Update your plugins

Run `fastlane update_plugins` to make sure to have all the latest plugins and their dependencies installed.

### Use `bundle exec`

Run `fastlane` using `bundle exec fastlane [lane]` to make sure your plugins are properly loaded.

This is required when you use plugins from a local path or a git remote.

### Use the `--verbose` mode

Running `fastlane [lane] --verbose` will show a lot more information that might be useful to resolve the issue.

### Make sure the action name is correct

A plugin can contain any number of actions. Make sure to read the docs for the plugin itself!

Additionally check out the source code of the plugin:

```
lib/fastlane/plugin/[plugin_name]/actions/[action_name].rb
```

Open the `[action_name].rb` file and make sure the name of the class on line 3 looks like this:

```
class [ActionName]Action < Action
```

Note how the name of the class should be capitalised and have `Action` appended in the name. Additionally this class must be a subclass of `Action`.

### Gemfile and Pluginfile

Your `Gemfile` should look something like this:

```ruby
gem "fastlane"

plugins_path = File.join(File.dirname(__FILE__), 'fastlane', 'Pluginfile')
eval(File.read(plugins_path), binding) if File.exist?(plugins_path)
```

Your `Pluginfile` should look something like this

```ruby
# Autogenerated by fastlane

gem 'fastlane-plugin-ruby'
```

### Building your own plugin

If you have issues with running your local plugins during plugin development, make sure to run

```
bundle install --with development
```

to install all required development dependencies

### More help

If it's still not working for you, please [submit a new GitHub issue](https://github.com/fastlane/fastlane/issues/new) with your `Gemfile`, `Gemfile.lock`, `Pluginfile`, `Fastfile` and terminal output when running `fastlane` using the `--verbose` flag.
