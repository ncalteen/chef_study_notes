Configuration Management and Chef
=================================

The set of engineering practices for managing and following entities involved in delivering software applications to consumers.
  - Hardware
  - People
  - Process
  - Software
  - Infrastructure

Requires coordination of many administrators, developers, and servers.

Changes must be handled in a systematic fashion to ensure that a system is configured in a correct and reliable manner.

There must be some form of autonomy in a system so that it can automatically detect faults and repair them without being explicitly told to do so.

Why You Need Configuration Management to Automate IT
----------------------------------------------------
- Consistency
- Efficient Change Management
- Simplicity in Rebuild Operations
- Visibility.

What is Chef?
-------------
An automation platform that configures and manages your infrastructure whether it is on premise or in the cloud.
- Your infrastructure is versionable.
- Your infrastructure is repeatable.
- Your infrastructure is auditable.
- Your infrastructure is testable.

Why Chef Might be a Good Tool for Your Enterprise
-------------------------------------------------
A configuration management tool should enable web IT, and provide support for managing cloud infrastructure.

Complex, enterprise infrastructures benefit from being able to model their IT infrastructure and application delivery processes as code.

Great tools and ideas come from community involvement.

Chef Can:
- Fully automate deployments, including internal development and end-user systems.
- Automate scaling of infrastructure.
- Make infrastructure self-healing.

Configure Your Chef Development Environment
===========================================
The ChefDK comes bundled with the native Ruby engine.

Chef Client vs. ChefDK
----------------------
Chef Client contains the core components of Chef needed to manage a server or workstation.
- It is installed on every node intended to be managed by Chef.

ChefDK is a superset of Chef Client.

Install the Chef Development Tools on OSX
-----------------------------------------
If there is no ChefDK for the version of OSX being used, you will need to install Chef Client along with the related tools.

ChefDK automatically installs Chef and Ruby in `/opt/chefdk/embedded`.

ChefDK includes a `chef shell-init` command to modify the current shell environment to use this path.
- The following command permanently enables this PATH setting.

  `$ echo ‘eval’ “$(chef shell-init bash)”’ >> \~/.bash\_profile`

If installing the Chef Client instead, Chef and Ruby will be located under `/opt/chef/embedded`.

You will want to add the bin directory in this path to your PATH.

Verify the Installation in OSX
------------------------------

Verify the Ruby engine being used is from the Chef installation.
- Should output `/opt/chefdk/embedded/bin/ruby` (ChefDK installation) or `/opt/chef/embedded/bin/ruby` (Chef Client installation).

Verify the core Chef binaries are included in `/opt/chefdk/bin` or `/opt/chef/bin`.
- chef-client
- ohai
- knife
- chef-solo
- etc.

Verify the Chef Client version.

`$ chef-client --version`

Install Test Kitchen in OSX (Chef Client Only)
----------------------------------------------
*Gem:* A supporting library or application written in Ruby.

Install Test Kitchen

`$ sudo gem install test-kitchen`

Optionally include the `--no-ri` and `--no-doc` parameters to save time by omitting the step which generates documentation.

You can verify the Ruby Gems installation directory with:

`$ gem env`

Run the below to verify Test Kitchen is installed.

`$ gem list test-kitchen –i`

If installed, this will return true.

Ruby and Chef Syntax
====================
Ruby is object-oriented, but supports functional and imperative programming paradigms.

Ruby is dynamically-typed.

Encourages consistency, common design patterns, and reusable code.

Ruby Syntax
-----------
You can use the below command to check a file’s syntax prior to executing the actual code.

`$ ruby –e /path/to/file`

Comments start with #

You do not need to declare the type of variable before assigning it.

Variables are accessible within the outermost scope of their declaration.

```
bacon_type = ‘crispy’

2.times do
  # Works!
  puts bacon_type
  temp = 300
end

# Will not work!
puts temp
```

Complex mathematical operations, such as rounding or polynomial distributions, are exposed in Ruby’s Math module.

```
Math.hypot(3,4) #=> 5
```

Single-quoted strings do not apply interpolation.

*Interpolation:* Evaluating variables as part of string content.

```
x = “hello”
“#{x} world” #=> “hello world”
‘#{x} world’ #=> “#{x} world”
```

Both double and single quoted strings use backslash as escape characters.

*Heredoc Notation:* Use of a string identifier for multi-line strings.

Supports interpolation.

```
<<METHOD_DESCRIPTION
This is a multiline string.
It supports #{variable} interpolation.
METHOD_DESCRIPTION
```

Arrays are zero-indexed and memory is automatically allocated as items are added or removed.

Hashes are key value pairs similar to JSON.
```
prices = { oscar: 4.55, bows: 5.23 }
prices[:oscar] #=> 4.55
prices.values #=> [ 4.55, 5.23 ]
```

*Symbol:* Immutable strings; constants.
```
key: value
```

Hash-Rocket Syntax
```
:key #=> value
```

Using strings as hash keys:
```
hash = { ‘key’ => value }
hash[‘key’] #=> value
```

Chef most commonly uses string keys for consistency.

Chef uses a custom data structure, called a Mash.

A mash does not care about the data type used to access a given element in a hash.
- The following are considered equivalent.

```
mash = Hashie::Mash.new({ key: value })
mash[:key] #=> value
mash[‘key’] #=> value
mash.key #=> value
```

Hashie::Mash is not part of the core Ruby library.

Regular expressions are evaluated using Perl-style operators.

```
“Bacon life” =~ /lie/ #=> nil
“Bacon life” =~ /bacon/ #=> 0
“Bacon life” =~ !/lie/ #=> true
```

The “if” conditional is complimented by the “unless” conditional.
- “unless” is equivalent to “if not”.

Multiple levels of conditionals can be nested with “elsif”.

When using a “case” statement:
- If a literal object is supplied in the “when”, a pure equality match is performed.
- If multiple items are given in the “when”, they are interpreted as “or”.

Ruby methods are defined using the “def” keyword.

Ruby classes are defined using the “class” keyword.

Ruby modules are defined using the “module” keyword.

Methods may be called with or without parenthesis, even when including variables.

```
my_method(5)
my_method 5
```

Chef Syntax and Examples
------------------------
Chef DSL (Domain Specific Language) is a subset of Ruby.

In Chef, resources are the building blocks used to define specific parts of your infrastructure.

```
resource ‘NAME’ do
  param1 value1
  param2 value2
end

template ‘/etc/resolv.conf’ do
  source ‘resolv.conf.erb’
  owner ‘root’
  group ‘root’
  mode ‘0644’
end
```

Chef uses a multiphase execution model, which lets you include logic switches or loops inside Chef code.

```
[ ‘bacon’, ‘eggs’, ‘sausage’ ].each do |type|
  file “/tmp/\#{type}” do
    content “Hello!”
  end
end
```

Chef will dynamically expand the loop.

Common Chef Resources
---------------------
*bash:* Execute multiline scripts using the bash shell interpreter.

```
bash ‘echo “hello”’
```

*chef_gem:* Install a gem inside Chef, for use inside Chef.
- Useful when Chef code requires a gem to function.

```
chef_gem ‘httproxy’
```

*cron:* Create or manage a cron entry.

```
cron ‘weekly-restore’ do
  weekday ‘1’
  minute ‘0’
  hour ‘0’
  command ‘sudo reboot’
end
```

*deploy_revision:* Control and manage a deployment of code from source control.

```
deploy_revision ‘/opt/myapp’ do
  repo ‘git://github.com/username/app.git’
end
```

*directory:* Manage a directory or directory tree, handling permissions and ownership.

```
directory ‘/opt/my\_dir’ do
  owner ‘root’
  group ‘root’
  mode ‘0644’
  recursive true
end
```

*execute:* Execute an arbitrary one line command.

```
execute ‘write status’ do
  command ‘echo “Hi” &gt; /tmp/hello’
end
```

*file:* Manage a file already present.

```
file ‘/tmp/hello’ do
  action :delete
end
```

*gem_package:* Install a gem for use outside Chef.

```
gem_package ‘bundler’
```

*group:* Create or manage a local group definition with local group members.

```
group ‘webusers’
```

*link:* Create or manage symlinks or hard links.

```
link ‘/tmp/begin’ do
  to ‘/tmp/end’
end
```

*mount:* Mount or unmount a file system.

```
mount ‘/dev/sda8’
```

*package:* Install a package using the OS’s underlying package manager.

```
package ‘apache2’
```

*remote_file:* Transfer a file from a remote location.

```
remote_file ‘/tmp/hello’ do
  source ‘http://hello.org/hi.tar.gz’
end
```

*service:* Start, stop, or restart a service.

```
service ‘apache2’ do
  action :restart
end
```

*template:* Manage plain-text file contents parsed as an Embedded Ruby template.

```
template ‘/tmp/bacon’ do
  source ‘bacon.erb’
end
```

*user:* Create or manage a local user account.

```
user ‘timmy’
```

Write Your First Chef Recipe
============================

*Recipe:* A file that contains Chef code.

***hello.rb:***

```
file ‘hello.txt’ do
  content ‘Hi everybody!’
end
```

*Resource:* Building blocks for assembling Chef code.
- A statement within a recipe that defines actions for Chef to perform.

*chef-apply:* A wrapper on top of Chef Solo.
- Allows you to run Chef code locally.

`sudo chef-apply ./hello.rb`

Examine hello.rb
----------------
Chef DSL describes what the desired configuration is, not how it should be achieved.

A file resource is created referring to the file hello.txt.

The file resource takes a string parameter specifying the path to the file.

The do…end clause denotes the beginning and the end of the statement block.

The content attribute specifies the string to the written to the file.

*Attribute:* A Chef variable used as a parameter to a resource.

Recipes Specify Desired Configuration
-------------------------------------
It is not safe to use implied relative paths with Chef resources.

```
file “\#{ENV\[‘HOME’\]}/stone.txt” do
```

Chef may run in different locations.

Chef prefers absolute paths to ensure consistency.

For file paths, Chef automatically converts “/” to “\\” for Windows OS.

Variables references in strings are denoted by `#{<variable>}`.

Variable references in strings must be enclosed in double quotes, otherwise the variable is not interpolated.

On Windows, Chef uses PowerShell to evaluate command-line references.
- More Unix-compatible.

If the resource does not exist, Chef will create and configure.

If the resource does exist, Chef will check for any configuration drift.
- *Configuration Drift:* When a resource is modified outside Chef, Chef will revert the resource back to the configuration defined by the recipe.

To Uninstall, Specify What Not to Do
------------------------------------
You can tell Chef explicitly what not to do.

Chef restricts itself in trying to reason about the state of a system only to the extent of what is explicitly defined in the recipe.
- To remove a file, you must explicitly tell Chef that the desired configuration is for the file not to exist.

```
file “#{ENV[‘HOME’]}” do
  action :delete
end
```

The file resource performs a `:create` action by default.

In Chef, a string prefaced by a “:” is a symbol (constant).

Manage Sandbox Environments with Test Kitchen
=============================================
Test Kitchen works in conjunction with Vagrant and VirtualBox to produce a local sandbox.

Introducing Test Kitchen
------------------------
Recommended to create a project directory for each sandbox environment.

`kitchen init--create-gemfile`

Generates all the config files needed to add Test Kitchen support to a project.

Use the `--create-gemfile` option to run `gem install` as an admin instead of a user.

You will be prompted to run `bundle install` after `kitchen init`.

Bundler is a tool that downloads and manages Ruby gems.

This will install kitchen-vagrant and other related gems.

kitchen-vagrant is a driver for Test Kitchen that adds support for managing VirtualBox and VMWare VMs using Vagrant.

*Current Directory Structure*
```
/
  .kitchen.yml
    - Used to configure virtual environments for Test Kitchen.
  Gemfile
    - Bundler uses this to configure the gem repository and the list of gems to download.
    - Bundler will automatically track dependencies, so you only have to include top-level gems.
  Gemfile.lock
    - Records all the versions of gems Bundler downloaded for the current project, plus versions for all dependencies.
    - Can be used by other developers to replicate an environment using ‘bundle install’.
  .kitchen/
    - Hidden directory Test Kitchen uses to store persistent data.
  .kitchen/logs/
    - Text outputs from the last run of Test Kitchen.
  test/
    - Used to contain tests.
    - Initialized as ./test/integration/default/
```

Spinning Up Your First Virtual Machine
--------------------------------------
*Instance:* An environment that includes a way to create a virtual machine and deploy automation code.

*kitchen create [name]:* Create a virtual environment specified in `.kitchen.yml`.
- If a name is not specified, all listed environments are created.
- Outputs will include connection details.

*Basebox:* A bare-bones OS installation, similar to a container.
- Test Kitchen is preconfigured to use baseboxes from Vagrant Cloud.
- Use `vagrant box list` to see available boxes.
- The Packer tool can be used to create custom baseboxes.

You can log into an instance using `kitchen login [name]`.

Test environments can be removed with `kitchen destroy [name]`.

YAML Overview
-------------
YAML files work with two kinds of data: key-value pairs and lists.

Key-value pairs can be nested.

```
driver:
  name: vagrant
  aws_access_key_id: 1234
```

YAML is sensitive to vertical alignment.

Lists are ordered, and values are associated with a key.

```
platforms:
- name: ubuntu_14.04
- name: ubuntu_16.04
```

YAML supports integers, strings, and arrays.

Test Kitchen Configuration with .kitchen.yml
--------------------------------------------
*Driver:* Specifies the driver plugin to use, plus configuration parameters to manage Test Kitchen environments.
- You can list available drivers with `kitchen driver discover`.
- Drop the ‘kitchen-‘ part of the name when specifying in `.kitchen.yml`.

*Provisioner:* Determines which configuration management tool will be used to provision the driver’s environment(s).
- When you run `kitchen setup`, it will install chef-client on the node.

*Platforms:* A list of operating systems for which Test Kitchen will create instances.

*Suites:* In the case of the Chef configuration management tool, specifies a configuration to be run on each instance.
- Among other things, a list of recipes to run on each instance.

Summary
-------
`kitchen init`: Add Test Kitchen support to a project.

`kitchen list`: Display information about Test Kitchen instances.

`kitchen create`: Start a Test Kitchen instance, if not already running.

`kitchen login`: Log into a Test Kitchen instance.

`kitchen destroy`: Shut down an instance and destroy the VM.

Managing Nodes with Chef Client
===============================
*Node:* A machine managed by Chef.
- Runs Chef recipes to ensure the machine is in a desired configuration.

Though ChefDK is superset of Chef Client, it is approximately double the footprint, and not appropriate for installation on managed nodes.

Create a New Sandbox Environment for a Node
-------------------------------------------
Create a project directory to contain your sandbox environment.

```
mkdir node
cd node
```

Generate the required Test Kitchen configuration and install supporting gems.

```
kitchen init --create-gemfile
bundle install
```

Modify `.kitchen.yml` to use the supported basebox.

***.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: chef_solo
platforms:
  - name: centos65
    driver:
    box: learningchef/centos65
    box_url: learningchef/centos65
suites:
  - name: default
    run_list:
    attributes:
```

Start the Test Kitchen instance.

```
kitchen create default-centos65
```

Installing Chef Client with Test Kitchen
----------------------------------------
Connect to the newly created node.

```
kitchen login default-centos65
```

Check if chef-client is installed.

```
chef-client --version
```

Initially this is not installed.

Log out of the node and install chef-client.

```
exit
kitchen setup default-centos65
```

The `kitchen setup` command runs the provisioner on the node.

*Provisioner:* A generic term for any kind of configuration management software.
- By default, Test Kitchen is configured to use Chef Solo as a provisioner.

Now, logging in and testing the chef-client version should return as expected.

```
kitchen login default-centos65
chef-client --version
exit
```

Your First Chef Client Run
--------------------------
The log resource can be used to print strings from a recipe.

```
kitchen login default-centos65
echo ‘log “Hello, World!”’ >> hello.rb
```

*Chef Run:* Using chef-client to execute actions in recipes.

```
chef-client --local-mode hello.rb --log_level info
```

The `--local-mode` parameter tells chef-client not to look for a Chef Server.

Log severity levels:
- debug
- info
- warn
- error
- fatal

Chef Client only prints messages of severity warn or greater unless otherwise specified.

Chef run logs can be directed to another destination with the `--logfile option`.

Chef Client Modes
-----------------
*Local Mode:* A full Chef Server is simulated in memory.
- Anything normally saved to a server is written to the local directory (writeback).

*Client Mode:* A Chef Server is running on another system.
- Chef Client runs as an agent on nodes.

*Solo Mode:* Provides a limited subset of Chef functionality running locally.
- Does not support writeback.
- Less convenient than local mode, and in the process of being phased out.

Ohai
----
Runs every time a Chef run is initiated.

Collects system information on nodes and stores as a set of automatic attributes.
- Network Config
- CPU State
- OS
- Memory Usage
- etc…

Chef converts ohai output into a node object, which is accessible by Chef code.

*Attribute:* A variable maintained by Chef.
- `node[‘ipaddress’]`, `node[:ipaddress]`, or `node.ipaddress`.
- `node[‘virtualization’][‘system’]`

Accessing Node Information
--------------------------
Access the node attribute to output system information.

```
cat >> EOF < info.rb
  log “IP: #{node[‘ipaddress’]}”
  log “MAC: #{node[‘macaddress’]}”
  log “OS: #{node[‘platform’]} #{node[‘platform_version’]}”
EOF
```

Running another chef-client in local mode will output the logged attributes.

Cookbook Authoring and Use
==========================
Each cookbook represents the set of instructions required to deploy a single unit of infrastructure.

Your First Cookbook
-------------------
Both `chef` and `knife` allow for creation of cookbooks.
- `knife` creates the entire directory structure at once.
- `chef` creates only what you specify incrementally, as well as the Berksfile.

```
chef generate cookbook motd
```

Update `.kitchen.yml` to use the below.

***.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: chef_solo
platforms:
  - name: centos65
    driver:
      box: learningchef/centos65
      box_url: learningchef/centos65
suites:
  - name: default
    run_list:
    attributes:
```

Use `chef` to generate files to be used within your recipe.

```
chef generate file motd
```

Use the name of the file you want to create, not the path.

Modify the generated file in `./files/default/motd` to contain a new message of the day.

Introducing the cookbook_file Resource
---------------------------------------
All recipe `.rb` files are expected to be in the recipes/ subdirectory of a cookbook.

The Linux message of the day can be replaced with the following recipe.

***default.rb***
```
cookbook_file “/etc/motd” do
  source “motd”
  mode “0644”
end
```

The cookbook_file resource is used to transfer files from a cookbook to nodes.

Performing Your First Converge
------------------------------
*Converge:* The process of deploying a cookbook to a node, running chef-client on the node, and applying a run list to put the node in a desired state.
- Plan for the steps Chef uses to configure a node as entirely data driven based on the output from ohai.

From within the cookbook directory, `./motd`:

```
kitchen converge default-centos65
```

The converge command will start the Test Kitchen node if needed.

It is logically similar to `kitchen create && kitchen setup`.

Anatomy of a Chef Run
---------------------
In production, chef-client is typically run in daemonized mode as a service on the node, performing Chef runs at regular intervals.

1.  Start the Chef Client
- Responsible for evaluating cookbooks on the target node.
2.  Build the Node Object
- The chef-client process constructs the node object in memory.
3.  Synchronize
- A run list is sent to the node.
- The node may also be sent a list of URLs of cookbooks to download that are required by the run list.
4.  Load
- Cookbook-level attributes are merged with automatic attributes.
- All files from the `libraries/` directory of every cookbook are loaded.
    - Attributes
    - Definitions
    - Resources
    - Providers
    - Recipes
- At this stage, recipes are not executed.
- However, Ruby code is executed and each resource is added to the resource collection.
5.  Converge
- Recipes are executed on the target node.
6.  Report
- Any new values in the node object are saved.
- If anything fails, new values are not saved and exception handlers are executed.

A run list specifies recipes in the form `recipe[‘<cookbook>::<recipe>’]`.

`recipe[‘<cookbook>’]` is equivalent to `recipe[‘<cookbook>::default’]`.

In production, the run list is maintained on Chef Server as a node attribute.

Cookbook Structure
------------------
```
./<cookbook>
  .kitchen.yml
  README.md
  attributes/
    default/
  chefignore
  files/
    default/
  metadata.rb
  recipes/
    default.rb
  templates/
    default/
```

*.kitchen.yml:* YAML configuration file for Test Kitchen.

*README.md:* Markdown-formatted text file for documentation.
- Allows for human readability with HTML-like styling.

*attributes/:* You can provide custom attributes to compliment, or override, the output generated by ohai.
- Define application distribution paths, platform-specific values, or software versions to install on a given node.
- Attributes files are evaluated in alphabetical order.

*chefignore:* The list of files to ignore when uploading the cookbook to Chef Server (when a Chef Server is being used).
- No need to upload editor swap files, source control tracking files, etc.

Cookbook Structure
------------------
*files/:* Centralized store for files to be distributed to nodes.
- Deployed using a cookbook_file resource.
- When files are to be distributed to all nodes, they are expected to be in `files/default/`.
- The directory structure under `files/` controls whether file are copied to particular nodes.

*metadata.rb:* All information about a cookbook.
- Must contain the name, version, dependencies, and other useful information.

*recipes/:* Contains Chef recipes and code.
- The main recipe must be `default.rb`.
- For each node, Chef evaluates only recipes specified in the nodes run list.

*templates/:* Holds Embedded Ruby (.erb) template files.
- Plain text files that can contain Ruby code.
- Ruby is evaluated before being rendered on the node.
- Follows the same directory conventions as `files/`.

Apache Cookbook
---------------
*Name:* Must be unique across your organization.
- Should be abbreviated statement of work.

*Purpose:* Must fully define scope of work to avoid scope creep in the future.
- Closely tied to the `metadata.rb` description attribute.

*Success Criteria:* Items to evaluate to determine if the cookbook meets its intended purpose.

*App/Service:* Each cookbook should manage a single app, service, or another component of your infrastructure.

*Required Steps:* Automation requires knowledge of the manual steps needed to achieve your goal.

Generate the Cookbook Skeleton
------------------------------
Generate a cookbook with Test Kitchen configuration.

```
chef generate cookbook <name>;
```

Using `knife cookbook create <name>` will not add Test Kitchen support.

```
chef generate cookbook apache
cd apache
```

Update `.kitchen.yml` to use our CentOS image.

***.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: chef_solo
platforms:
  - name: centos65
    driver:
    box: learningchef/centos65
    box_url: learningchef/centos65
suites:
  - name: default
    run_list:
    attributes:
```

Verify there are no syntax errors.

```
kitchen list
```

Introducing the Package Resource
--------------------------------
Run `kitchen converge` frequently to validate your syntax.

```
kitchen converge default-centos65
```

Install Apache using the package resource in `recipes/default.rb`.

```
package ‘httpd’ do
  action :install
end
```

Run `kitchen converge` to run the updated recipe.

Log into the Test Kitchen instance and check if Apache is installed.

```
kitchen login default-centos65
rpm -q httpd
```

Since `:install` is the default action for the package resource, we can simplify the above recipe.

```
package ‘httpd’
```

Introducing the Service Resource
--------------------------------
Can be used to start a service, as well as configure automatic start on reboot.

***recipes/default.rb***
```
package ‘httpd’

service ‘httpd’ do
  action [ :enable, :start ]
end
```

Multiple resources in the same recipe are evaluated in the order listed.

```
kitchen converge default-centos65
kitchen login default-centos65
chkconfig --list | grep 3:on
```

Introducing the Template Resource
---------------------------------
First, add the resource to your recipe.

***recipes/default.rb***
```
package ‘httpd’

service ‘httpd’ do
  action [ :enable, :start ]
end

template ‘/var/www/html/index.html’ do
  source ‘index.html.erb’
  mode ‘0644’
end
```

The template is expected to be in the `templates/` directory, following the same subdirectory convention as `files/`.

Chef allows you to select the most appropriate file in a cookbook according to platform filters (specified as the folder name under `templates/` or `files/`).
- Host node name (foo.bar.com).
- Platform version (redhat-6.5.1).
- Platform (redhat).
- Default.

Use `chef generate template <filename>` to create your template file.

```
chef generate template index.html
```

Variables bounded by `<%= [variable] %>` are expanded by the embedded Ruby engine.

***templates/default/index.html.erb***
```
This site was set up by <%= node[‘hostname’] %>!
```

```
kitchen converge default-centos65
kitchen login default-centos65
more /var/www/html/index.html
curl localhost
```

Verify Success Criteria Are Met
-------------------------------
Need to assign a known, static IP to your node in `.kitchen.yml` in order to be able to test from outside Test Kitchen.

```
driver:
  network:
  - [“private_network”, {ip: “192.168.1.11”}]
```

Test Kitchen will apply network configuration settings only when running `kitchen create` for the first time.

```
kitchen destroy default-centos65
kitchen converge default-centos65
curl 192.168.1.11
```

Summary
-------
*package:* Installs a package using the system package manager.

*service:* Maintains the lifecycle of any services installed by the package resource.

*cookbook_file:* Transfers a file from the `files/` directory of a cookbook to a path on the node.

*template:* A variant of the cookbook_file resource that lets you create file content from variables in an ERB template.

Cookbook Process
----------------
1.  Define prerequisites and goals.
2.  Generate the cookbook skeleton.
3.  Let the documentation you write in `README.md` guide development.
4.  Define `metadata.rb`.
5.  Verify cookbook code as you write using `kitchen converge` and `kitchen login`.
6.  Verify conditions of success are met.

Attributes
==========
Attributes represent information about your node.

Includes information generated by ohai.

Includes any attributes set in recipes or attribute files.

Located in `./<cookbook>/attributes/default.rb` (or other .rb files).

```
# Set in attributes file.
default[‘apache’][‘dir’] = “/etc/apache2”
# Set in recipe file.
node.default[‘apache’][‘dir’] = “/etc/apache2”
```

In the above example, the precedence is defined as “default”.
All attribute values are composed together in a Chef run based on priority.

| Priority         | Type                       |
| ---------------- | -------------------------- |
| Highest Priority | Automatic (Ohai)           |
|                  | Defined by Recipe          |
| Lowest Priority  | Defined by Attribute Files |

Motd-Attributes Cookbook
------------------------
Generate a cookbook and update `.kitchen.yml`.

```
chef generate cookbook motd-attributes
```

***.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: chef_solo
platforms:
  - name: centos65
    driver:
      box: learningchef/centos65
      box_url: learningchef/centos65
suites:
  - name: default
    run_list:
    attributes:
```

Create a recipe that uses a template resource to generate a new `/etc/motd` file.

***recipes/default.rb***
```
template ‘/etc/motd’ do
  source ‘motd.erb’
  mode ‘0644’
end
```

Generate a template file that uses some node attributes set by ohai.

```
chef generate template motd
```

***templates/motd.erb***
```
The hostname of this node is <%= node[‘hostname’] %>
The IP of this node is <%= node[‘ipaddress’] %>
```

```
kitchen converge default-centos65
kitchen login default-centos65
more /etc/motd
```

Setting Attributes
------------------
First, generate a default attributes file.

```
chef generate attribute default
```

Now you can set attributes in `./<cookbook>/attributes/default.rb`.

***attributes/default.rb***
```
default[‘motd-attributes’][‘company’] = ‘Chef’
```

When attributes are set in a cookbook’s attributes file, the values are expected to be namespaced under a top-level key matching the name of the cookbook.

We can also set an attribute value directly in our recipe.

***recipes/default.rb***
```
node.default[‘motd-attributes’][‘message’] = “Hello, world!”
```

Now, both attibutes can be reference in our template file.

***templates/motd.erb***
```
Welcome to <%= node[‘motd-attributes’][‘company’] %>
<%= node[‘motd-attributes’][‘message’] %>
The hostname is <%= node[‘hostname’] %>
The IP is <%= node[‘ipaddress’] %>
```

```
kitchen converge default-centos65
kitchen login default-centos65
more /etc/motd
```

Basic Attribute Priority
------------------------
Test modifying the default recipe so it resets values set elsewhere.

***recipes/default.rb***
```
# Reset a higher priority attribute.
node.default[‘ipaddress’] = “1.1.1.1”
# Reset a lower priority attribute.
node.default[‘motd-attributes’][‘company’] = “Amazon”
```

```
kitchen converge default-centos65
kitchen login default-centos65
more /etc/motd
```

Note the company value set in the recipe has a higher priority than the value set in the attributes file, so it is overwritten.

Note the IP address value set in the recipe has a lower priority than the attribute set by ohai, so it is not overwritten.

include_recipe
---------------
A Chef recipe can reference other recipe files using an include_recipe statement.

The files can be in separate recipes, or even separate cookbooks.

Chef code might contain conflicting attribute assignments.

Attribute prioritization applies guidelines on how conflicts are resolved.

A good rule of thumb is that a single recipe should be no longer than your screen (18-30 lines).

Break apart long recipes and stitch together with include_recipe statements.

Generate a new recipe called message.

```
chef generate recipe message
```

Update the new recipe to include an attribute definition.

***recipes/message.rb***
```
node.default[‘motd-attributes’][‘company’] = “My Company”
```

For private recipes, its preferred to prefix the recipe with an underscore.

This shows that the recipe is not referenced directly in run lists.

Update the default recipe to include the message recipe.

Make sure to do so after the attribute definitions added previously.

***recipes/default.rb***
```
# Attribute definitions.
include_recipe ‘motd-attributes::message’
# Resource definitions.
```

Include statements can be present anywhere in a recipe file.
- Include statements are replaced with an expansion of the referenced recipe code in the same location.

When testing, you will see that, if duplicate attribute values are set at the same priority value, the last one set “wins”.

```
kitchen converge default-centos65
kitchen login default-centos65
more /etc/motd
```

Attribute Precedence
--------------------
The commonly used levels of precedence in attribute definitions:
- *Automatic:* Attributes discovered by ohai.
- *Default:* Attributes set in cookbooks and cookbook files.
- *Override:* Strongest way to set attributes. Not recommended to use unless absolutely necessary.

Automatic > Override > Default

Debugging Attributes
--------------------
The `node.debug_value()` method allows you to debug where attributes are set.
- Returns the raw contents of an object.
- Use pretty print `pp` for a readable format.

Update `default.rb` to debug the IP address attribute.

***recipes/default.rb***
```
require ‘pp’

node.default[‘ipaddress’] = ‘1.1.1.1’

pp node.debug_level(‘ipaddress’)
# Remainder of recipe code.
```

```
kitchen converge default-centos65
```

During the Chef run, you will see that Chef did register the update at the default precedence, but the automatic value set by ohai “won”.

Nested attributes can be evaluated as well.

```
node.debug_value(‘parent’, ‘child’)
node.debug_value(‘parent’, ‘child’, ‘grandchild’, …)
```

It is helpful in debugging to use this method before and after include_recipe calls to see if attributes are modified within a recipe without having to search through code.

***recipes/default.rb***
```
pp node.debug_value(‘motd-attributes’, ‘company’)

include_recipe ‘motd-attributes::message’

pp node.debug_value(‘motd-attributes’, ‘message’)
# Remainder of recipe code.
```

```
kitchen converge default-centos65
```

You will see now that the outputs for each `node.debug_value()` invocation are different.

Summary
-------
Attributes can be set:
- Automatically by ohai.
- In attribute files.
- In recipes.
- In other cookbooks.

It is recommended to always use default priority unless otherwise specified.

Chef Server
===========

*Hosted Enterprise Chef:* SaaS implementation of Chef.
- Requires no server-level setup or configuration.

*On Premise Enterprise Chef:* Designed and deployed inside an organization.
- Includes additional features on top of Hosted Enterprise Chef.
- Useful for organizations with specific audit and compliance regulations.

*Open Source Chef Server:* Open source version of Chef Server.
- Contains a subset of Enterprise Chef Server features.

Stores and indexes cookbooks, environments, template, metadata, files, and policies.

Aware of all machines it manages.

Written in Erlang, and designed for concurrency.

Composed of a web server, cookbook store, web interface, messaging queue, and backend database.

*Web Server:* Simple reverse proxy acting as a front end to Chef Server.
- Performs load balancing for Chef Server.
- All requests to the Chef Server API are routed through nginx.

*WebUI:* Consumer-facing web application, built in Ruby on Rails.
- Web-guided interface for interacting with Chef Server.

*Chef Server:* Erchef is the component of Chef Server that processes API requests.
- Written in Erlang, but capable of running Ruby code.
- Writing Erlang Chef recipes is not supported.
- Erchef is the core API for Chef Server.

*Bookshelf:* Central store for all cookbooks and their contents.
- Each cookbook is automatically checksummed and versioned.
- Flat file database intentionally stored outside Chef Server's index.

*Search Index:* Apache Solr server that handles indexing and searching mechanism for various API calls, bot internally and externally.
- Wrapped by chef-solr, which exposes a RESTful API.

*Message Queues:* Handles all messages sent to the search index for parsing.
- Managed by RabbitMQ, an open source queueing service.
- chef-expander pulls messages from the queues, formats, and sends to the search index.

*Database:* PostgreSQL persistent data store.
- Prior to Chef 11, was running CouchDB, but this was unable to efficiently scale.

Installing Chef Server Manually
-------------------------------
Free tier of Enterprise Chef Server allows for management of up to 5 nodes.

Download the RHEL 6 version (this will be installed on the CentOS Test Kitchen instance).

Installing Enterprise Chef Server
---------------------------------
To maintain consistency with Hosted Enterprise Chef, create the chef-repo/cookbooks directory, and then the enterprise cookbook.

```
mkdir -p chef-repo/cookbooks
cd chef-repo/cookbooks
chef generate cookbook enterprise-chef
cd enterprise-chef
```

Edit `.kitchen.yml` to use the existing CentOS basebox.

This time, assign a private IP to the box.

Since Enterprise Chef requires more than the default 512MB of memory allocated to Test Kitchen instances, we can modify this setting using a `customize:` block.

Additionally, make sure to specify chef_solo provisioner, as the in-memory Chef Zero provisioner will cause issues with the Chef Server deployment.

***.kitchen.yml***
```
drivers:
  name: vagrant
provisioner:
  name: chef_solo
platforms:
  - name: centos65
    driver:
      box: learningchef/centos65
      box_url: learningchef/centos65
      network:
      - ["private_network", {ip: "192.168.33.34"}]
      customize:
        memory: 1536 # Value in MB
suites:
  - name: default
    run_list:
      - recipe[enterprise-chef::default]
    attributes:
```

Generate a default attributes file.

```
chef generate attribute default
```

Specify the download URL of the Chef Server package obtained from chef.io. For this exercise it is recommended to use version 11.1.8.

***attributes/default.rb***
```
default['enterprise-chef']['url'] = '<download_link>'
```

Optionally, you can instead download the file and store it as a file resource in your cookbook. This will copy the file to the Test Kitchen instance for deployment.

Develop the recipe by replicating the manual installation steps.

Use temporary variables to reduce the amount of typing.

```
package_url = node['enterprise-chef']['url']
```

Extract the package name from the package_url using built-in Ruby functionality. The `::File.base_name` method returns the last component of the URL after the final forward slash.

```
package_name = ::File.base_name(package_url)
```

The package resource does not work with files stored remotely.
- You will need to download the file using the remote_file resource.
- Rather than hardcode a download path file like `/tmp`, Chef can instead choose the best place to store temporary files for you.

```
package_local_path = "#{Chef::Config[:file_cache_path]}/#{package_name}"

remote_file package_local_path do
  source package_url
end

package package_local_path
```

Use the execute resource to run a shell command to configure the Chef Server.

```
execute 'chef-server-ctl reconfigure'
```

***recipes/default.rb***
```
package_url = node['enterprise-chef']['url']
package_name = ::File.base_name(package_url)
package_local_path = "#{Chef::Config[:file_cache_path]}/#{package_name}"

remote_file package_local_path do
  source package_url
end

package package_local_path

execute 'chef-server-ctl reconfigure'
```

Test this recipe.

```
kitchen converge default-centos65
```

Introducing Idempotence
-----------------------
The new Chef Server installation recipe works, but it is not idempotent.
- It does run successfully on the same node multiple times, but will modify the same resources for no reason.

All Chef default resources are generated to be idempotent with the exception of the execute resource.

A good way to verify this is to run `kitchen converge` twice.
- There should be no unintended side effects, and 0 updated resources on the second run.

When installing a package from a local file, you need to explicitly tell the package resource to use the Chef::Provider::Package::Rpm provider using the provider attribute.

```
package package_name do
  source package_local_path
  provider Chef::Provider::Package::Rpm
end
```

You can also use the rpm_package short name to specify the provider directly.

```
rpm_package
  source package_local_path
end
```

You can use not_if to guard the execute resource.
- Allows the resource to test for a desired state and, if present, do nothing.
- Test if the exit code of the command is 0.

```
execute 'chef-server-ctl reconfigure' do
  not_if 'rpm -q chef-server'
end
```

You can trigger events in other resources with a notifies statement.

The execute resource must do nothing by default, so that it is only invoked when needed.
- Additionally, the actual command should be moved to inside the resource so that the resource can be referenced by name.

```
execute 'chef-server-ctl reconfigure' do
  command 'chef-server-ctl reconfigure'
  action :nothing
end
```

The notifies statement takes three parameters: an action, the resource to notify, and a timer indicating when the action should be performed.

```
rpm_package package_name do
  source package_local_path
  notifies :run, 'execute[chef-server-ctl reconfigure]', :immediately
end
```

***recipes/default.rb***
```
package_url = node['enterprise-chef']['url']
package_name = ::File.base_name(package_url)
package_local_path = "#{Chef::Config[:file_cache_path]}/#{package_name}"

remote_file package_local_path do
  source package_url
end

rpm_package package_name do
  source package_local_path
  notifies :run, 'execute[chef-server-ctl reconfigure]', :immediately
end

execute 'chef-server-ctl reconfigure' do
  command 'chef-server-ctl reconfigure'
  action :nothing
end
```

Lastly, update your Test Kitchen instance.

```
kitchen converge default-centos65
```

Configure Enterprise Chef Server
--------------------------------
At this point you should be able to access your Chef Server by private IP. Open in the browser, where you will see initial login credentials. You will be prompted to download your `<org>-validator.pem` and starter kit.
- Additionally, you should select your username and `regenerate private key` to save your `<username>.pem` key.

If using a later version of Chef Server, you may need to install chef-manage.

```
kitchen login default-centos65
sudo chef-server-ctl install chef-manage
sudo chef-manage-ctl reconfigure --accept-license
```

As a separate task, you can update `recipes/default.rb` to include this modification. Additionally, later versions of Chef Server require setting up the first user account via email. This can instead be done directly on the Test Kitchen instance.

```
kitchen login default-centos65
sudo chef-server-ctl user-create <username> <first> [<middle>] <last> <email> '<password>'
```

With all three files downloaded, create your `.chef/` directory. Newer Chef Server versions handle this automatically with the starter kit.

```
cd chef-repo
mkdir .chef
```

*<username>.pem:* A unique identifier to authenticate your user against Chef Server.

*<org>-validator.pem:* Authenticates your organization against Chef Server.
- Unlike `<username>.pem`, this must be shared to anyone who needs access to your Chef organization.

Chef Server requires a valid FQDN set up in local DNS.
- For testing, you can add a local host entry instead of modifying DNS.

```
sudo sh -c "echo '192.168.33.34 default-centos65' >> /etc/hosts"
```

Generally your `knife.rb` will not contain sensitive data, but will be specific to users.

Testing the Connection
----------------------
The command `knife client list`, or any other list command, will verify the knife configuration.
- Initially, you may need to run `knife ssl fetch` to add unverified certificates to your knife configuration (removing the SSL verification error).

Bootstrapping a Node
--------------------
The process by which a remote system is prepared to be managed by Chef.

Create a Test Kitchen project for your node.

```
mkdir ./chef-repo/cookbooks/node
cd node
```

Initialize the directory as a Test Kitchen project.

```
kitchen init --create-gemfile
bundle install
```

Update `.kitchen.yml` to include a private IP. Additionally, include a synced folder to ensure one or more directories are kept in sync between the host and guest OS (this will be used later).

***.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: shell
platforms:
  - name: centos65
    driver:
      box: learningchef/centos65
      box_url: learningchef/centos65
      network:
      - ["private_network", {ip: "192.168.33.35"}]
      synced_folders:
      - ["../../../chef-repo", "/chef-repo"]
suites:
  - name: node
    attributes:
```

Now your node can be created.

```
kitchen create node-centos65
```

When running `knife bootstrap` on a workstation, it installs Chef Client on the node and registers it with Chef Server.

Nodes must also have fully-qualified domain names.

```
sudo sh -c "echo '192.168.33.35 node-centos65' >> /etc/hosts"
```

You will also need to configure the local DNS name of the Chef Server on the Test Kitchen node being registered.

```
kitchen login node-centos65
sudo sh -c "echo '192.168.33.34 default-centos65' >> /etc/hosts"
```

Now the node can be registered.

```
knife bootstrap --sudo --ssh-user vagrant --ssh-password vagrant --no-host-key-verify -N node-centos65 node-centos65
```

Verify the node was added on the Chef Server console UI. All ohai data for each node is stored on Chef Server.

Currently Test Kitchen does not have a way to pause virtual machines.
- In the project directory, run `vagrant halt` and `vagrant reload` from the projects' Vagrantfile directory.
- This can be used if you need to pause between sections of the material.

```
cd ./chef-repo/cookbooks/node/.kitchen/kitchen-vagrant/node-centos65
vagrant halt
vagrant reload
```

Bootstrap with Chef Solo
------------------------
You can use Chef Solo to deploy Chef Server outside of Test Kitchen.
- Does not cause the same "script confusion" as Chef Zero when deploying.
- Check out solowizard.com, a web form to generate a Mac OSX development workstation.

To bootstrap Chef Server with Chef Solo:
1. Install Chef Client.
2. Create the below directories for chef-solo state information and cookbooks.
```
/var/chef/cache
/var/chef/cookbooks
```
3. Copy any needed cookbooks.
4. Run chef-solo.

Summary
-------
On subsequent converges, idempotent recipes should never update the same resource twice.

Community and the Chef-Client Cookbook
======================================
Access to cookbooks, knife plugins, and community forums.

For this section, ensure that the node and Chef Server from the previous section are running.

```
cd ./chef-repo/cookbooks/enterprise-chef
kitchen converge
cd ./chef-repo/cookbooks/node
kitchen converge
knife bootstrap --sudo --ssh-user vagrant --ssh-password vagrant --no-host-key-verify -N node-centos65 node-centos65
```

Create a user and org, and download the starter kit.

Copy the `.chef/` directory.

```
cd ./chef-repo
knife ssl fetch
```

Update the node's host file to contain an entry for the Chef Server.

```
kitchen login node-centos65
sudo sh -c "echo '192.168.33.34 default-centos65' >> /etc/hosts"
```

Using Community Cookbooks
-------------------------
Always independently verify the behavior and flexibility of a cookbook on an isolated environment before using it in production!

Chef-Client Cookbook
--------------------
Configures chef-client to run as a service or cron job. Bootstrapping with `knife bootstrap` performs none of the actual chef-client setup required to run as a service/cron job.

Deletes the `validation.pem` file. The `validation.pem` key, or `<org>-validator.pem`, is an organization-wide private key specifically used to register new nodes with Chef Server on their first chef-client run.
- This can (and should) be removed after a node is registered successfully.

By default, the node is responsible for scheduling and initiating a chef-client run, as well as any related processing.
- Chef Server is more or less a cookbook repository and artifact metadata management service.

Knife Cookbook Site Plugin
--------------------------
Included in all recent versions of Chef Client and ChefDK.
- Command line interaction with Chef Supermarket.

Search for Community Cookbooks
------------------------------
`knife cookbook site search <parameter>`

Once an interesting cookbook is found, additional details can be searched.

`knife cookbook site show <cookbook>`

```
knife cookbook site search chef-client
knife cookbook site show chef-client
```

Manage Chef Supermarket Cookbooks on Your Chef Server
-----------------------------------------------------
1. Download the cookbook using `knife cookbook site download`.
2. Extract with `tar xvf`.
3. Upload the cookbook to your Chef Server with `knife cookbook upload`.
4. Repeat steps 1-3 for any dependencies.

```
knife cookbook site download chef-client [version]
tar xvf chef-client*.tar.gz -C cookbooks/
knife cookbook upload chef-client --cookbook-path <path>
```

If you attempt to upload now, you will get dependency errors. The chef-client cookbook depends on cron, logrotate, and windows cookbooks. Download and unzip these, then upload to Chef Server. Lastly, verify the cookbooks uploaded to Chef Server.

```
knife cookbook list
```

Chef-Client Recipes
-------------------
Now we can add chef-client to our node's run list.

*chef-client::default:* Configures chef-client to run as a service.

*chef-client::delete_validation:* Deletes the organization validation file.

```
knife node run_list add node-centos65 "recipe[chef-client]"
knife node run_list add node-centos65 "recipe[chef-client::delete_validation]"
```

Recipes are executed in the order specified in the run list.

At this point, you can now SSH into your Test Kitchen instance and perform a Chef run.

```
cd ./chef-repo/cookbooks/node
kitchen login node-centos65
sudo chef-client
```

Verify the validator key is deleted and chef-client is running as a daemon.

```
ls /etc/chef
ps awux | grep chef-client
exit
```

Configure knife to Use a Production SSL Setup
-----------------------------------------
On the node, SSL verification is controlled through settings in `/etc/chef/client.rb`.
- This can be enabled by settings in the chef-client cookbook.

Cookbooks should change behavior based on attributes. Well written cookbooks:
- Change behavior based on attributes.
- Shouldn't have to be modified to accommodate environment needs.
- Have sane default attributes, and an explanation of each attribute's effect on cookbook behavior (in `README.md`).

You will need to make sure that the Chef Server is configured to used the desired certificate.
- Chef server automatically generates a self-signed certificate during installation.
- The process for using a third-party certificate is well documented online.

Use `knife ssl check` to see what needs to be done next.
- The command output provides an explanation of current and desired state.
- By default, Chef does not trust self-signed certificates.
- They must be added to `./chef-repo/.chef/trusted_certs` using `knife ssl fetch`.
- In this example, you will need to ensure `/etc/hosts` contains an entry for your Test Kitchen Chef Server (default-centos65.vagrantup.com).

The chef-client cookbook includes a recipe, `chef-client::config`, which can be used to generate the `/etc/chef/client.rb` configuration file with the desired SSL settings.
- Allows for automatic SSL configuration of your nodes.

View your node's existing settings.

```
kitchen login
cat /etc/chef/client.rb
```

Bootstrapping a node with knife sets the following in `client.rb`.
- chef_server_url
- validation_client_name
- node_name
- trusted_certs_dir

To verify a self-signed certificate, we need to update `/etc/chef/client.rb` to include:

```
ssl_verify_mode    :verify_peer
```

The default setting is `:verify_none`. We can do this by setting the appropriate attribute for `chef-client::config`.
1. Open the Chef Server site.
2. Select Nodes -> node-centos65.
3. Select Actions -> Edit Attributes.
4. Add the following attribute JSON.

```
{
  "chef_client": {
    "config": {
      "ssl_verify_mode": ":verify_peer"
    }
  }
}
```

This can also be done on the command line using `knife node edit`, and then verified with:

```
knife node show --attribute "chef_client.config.ssl_verify_mode" node-centos65
```

Since we are using a self-signed certificate, we need to tell the SSL library on the node to trust it.
- In production, you would write a recipe to add the custom certificate to the cert store.
- If using OpenSSL, you will need to copy the certificate to `SSL_CERT_DIR` and run `c_rehash` to register the self-signed certificate.
- In this example, the synchronized folder we set up for the node makes the certificate in `./chef-repo/.chef/trusted_certs/` available locally on the node.

Add the ssl_ca_file attribute to the node under chef_client.config.

```
"ssl_cal_file": "/chef-repo/.chef/trusted_certs/default-centos65_vagrantup_com.crt"
```

Verify the configuration.

```
knife node show --attribute "chef_client.config.ssl_verify_mode" node-centos65
knife node show --attribute "chef_client.config.ssl_ca_file" node-centos65
```

Now that all settings are verified, add `chef-client::config` to the node's run list.

```
knife node run_list add node-centos65 "recipe[chef-client::config]"
```

Finally, run chef-client on the node. You will get one more SSL warning in the output, as the SSL settings have not yet been applied. Run chef-client another time, and you will see no further SSL warnings.

Chef Zero
=========
The ChefDK and Chef Client include a stripped down Chef Server, chef-zero.

Runs on as little as 20MB memory.

No WebUI or persistence.

Test Kitchen provides built-in support.

If using Chef Client, not ChefDK, you will need to install the chef-zero gem.

```
sudo gem install chef-zero --no-ri --no-rdoc
```

Test Kitchen and Chef Zero
--------------------------
Generate a cookbook and enable it for Test Kitchen

```
chef generate cookbook zero
cd zero
```

Edit the `provisioner:` stanza in `.kitchen.yml` to use chef_zero.

***.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: chef_zero
platforms:
  - name: centos65
    driver:
      box: learningchef/centos65
      box_url: learningchef/centos65
      network:
      - ["private_network", {ip: "192.168.33.34"}]
suites:
  - name: default
    run_list:
      - recipe[zero::default]
    attributes:
```

Perform a run using `kitchen converge`. Chef Zero only runs when chef-client is running. By default, Chef Zero does not configure knife for interacting with Chef Server.

Running Chef-Zero on Your Host Using Chef-Playground
----------------------------------------------------
Chef-Playground allows us to use knife, data bags, and search on a local workstation.

Create the chef-playground directory.

```
mkdir chef-playground
cd chef-playground
```

Create the `.chef/` directory in chef-playground to hold configuration and keys.

```
mkdir .chef
cd .chef
```

Generate some fake client keys. They do not need to be real keys tied to a specific Chef user.

```
ssh-keygen -f devhost.pem -P ""
ssh-keygen -f validation.pem -P ""
```

Generate your `knife.rb` configuration file.

***knife.rb***
```
chef_repo = File.join(File.dirname(__FILE__), "..")
chef_server_url   "http://127.0.0.1:9501"
node_name         "devhost"
client_key        File.join(File.dirname(__FILE__), "devhost.pem")
cookbook_path     "#{chef_repo}/cookbooks"
cache_type        "BasicFile"
cache_options     :path => "#{chef_repo}/checksums"
```

Run chef-zero in a separate terminal window, updating the running port to match the one configured in `knife.rb`. You can run in daemonized mode by passing the `--daemon` parameter.

```
chef-zero --port 9501 [--daemon]
```

Knife commands should now return data.

```
knife client list
```

Knife supports a `--local-mode` option. This will tell knife to automatically start chef-zero for you.

Pre-populate chef-zero with some node info to get back useful results.

```
mkdir nodes
cd nodes
```

***atwood.json***
```
{
  "name": "atwood",
  "chef_type": "node",
  "json_class": "Chef::Node",
  "chef_environment": "_default",
  "run_list": [
    "recipe[apache]",
    "recipe[motd]"
  ],
  "automatic": {
    "ipaddress": "192.168.33.31",
    "hostname": "atwood",
    "fqdn": "atwood.playground.local",
    "os": "linux",
    "os_version": "2.632-431.el6.x86_64",
    "platform": "centos",
    "platform_version": "6.5",
    "platform_family": "rhel",
    "recipes": [
      "apache",
      "motd"
    ]
  }
}
```

Upload the node data to chef-zero. This must be done from the chef-playground working directory.

```
knife upload nodes
knife node list
```

Search
======
Query data indexed on Chef Server. Queries can be invoked using knife or from within a recipe.

Search from the Command Line
----------------------------
Copy the chef-playround directory from the previous section.

Start chef-zero on an open port in one terminal window.

```
chef-zero --port 9501
```

From another terminal window, populate Chef Zero with your node information.

```
cd ./chef-playround
knife upload nodes
knife node list
```

Searching with knife can be performed in any environment.

```
knife search <index> <query>
```

The index can be one of the following:
- node
- client
- environment
- role
- <data_bag>

Search for nodes with an IP starting with 192.

```
knife search node 'ipaddress:192.*'
```

Chef search queries use Solr `'<attribute>:<pattern>'` form. Use an asterisk to perform wildcard searches.

```
knife search node 'platform*:centos'
```

Use a question mark to match any single character.

```
knife search node 'platform_version:14.0?'
```

Search in a Recipe Using Test Kitchen
-------------------------------------
Chef provides a `search()` method that you can use in your Chef code.

```
search(<index>, <query>)
```

***recipes/default.rb***
```
search("node", "*:*").each do |matching_node|
  log matching_node.to_s
end
```

The `do...end` block can be used to define a method with no name. You can pass an unnamed method one or more parameters enclosed by pipe characters. You can pass code blocks to iterators to execute a specific method for each item.

```
(0..5).each do |counter|
  puts counter
end
```

Run `kitchen converge default-centos65` and note the output.

```
* log [node[default-centos65]]
* log [node[atwood]]
```

The additional entry is the sandbox node Test Kitchen created to run the converge. The sandbox node is automatically registered with its own Chef Zero instance.

Summary
-------
Chef Server uses Apache Solr to add support for searching and indexing. You can search from anything that can talk to Chef Server. Search is implemented as an API. Node attributes can only be read by the node that created them, not by other nodes. Data bags are used to get around this limitation.

Data Bags
=========
Store shared, global dat between nodes.

A container for items that represent information about your infrastructure that is not tied to any single node.
- Shared passwords.
- Shared lists of users or groups.
- License keys.

There is no way for nodes to directly access attribute data stored in other nodes.

Each data bag contains a list of items.
- Each item is a JSON name-value pair collection expected to have exactly the same schema for every item in the data bag.
- The schema between data bags can differ.
- String values are quoted, integers are not.
- Values can contain lists of strings or integers.

Basic Command Line Data Bag Usage with Knife
--------------------------------------------
As an example, we want to ensure two users have accounts created on nodes in the environment, so we store their information in a data bag. The users are global and must be shared across nodes.

Copy the chef-playground directory from the previous example, then start Chef Zero in a separate terminal window.

```
chef-zero --port 9501
```

Create a data_bags directory, and a users data bag (also a directory).

```
mkdir -p data_bags/users
```

***data_bags/users/alice.json***
```
{
  "id": "alice",
  "comment": "Alice J.",
  "uid": 2000,
  "gid": 0,
  "home": "/home/alice",
  "shell": "/bin/bash"
}
```

***data_bags/users/bob.json***
```
{
  "id": "bob",
  "comment": "Bob F.",
  "uid": 2001,
  "gid": 0,
  "home": "/home/bob",
  "shell": "/bin/bash"
}
```

Create the data bag on Chef Zero server running in the background. Since we are using Chef Zero, you will need to manually add data bag items.

```
knife data_bag create users
knife data_bag from file users ./data_bags/users/alice.json
knife data_bag from file users ./data_bags/users/bob.json
```

Verify the items were added.

```
knife search users "*:*"
```

All search variants available for other attributes are available for data bags.
- AND
- OR
- Filter with -a

If the search text is a string, you will need to enclose in double quotes.

```
knife search users 'comment:"Alice J."'
knife search users "comment:\"Alice J.\""
```

Creating Local Users Based on Data Bag Items in a Recipe
--------------------------------------------------------
Now we can write a cookbook that creates users from data bags.

```
cd chef-playground/cookbooks
chef generate cookbook users
cd users
kitchen init --create-gemfile
bundle install
```

Add the data bags to the provisioner stanza of `.kitchen.yml`.

```
provisioner:
  data_bags_path: ../../data_bags
```

All files in `data_bags/` will be added to Chef Zero as data bag items. In production, you would not want to package data bags with your cookbooks!

You can use `search()` to perform a data bag query.

***recipes/default.rb***
```
search("users", "*:*").each do |user_data|
  user user_data["id"] do
    comment user_data["comment"]
    uid user_data["uid"]
    gid user_data["gid"]
    home user_data["home"]
    shell user_data["shell"]
  end
end
```

Run `kitchen converge default-centos65` and verify the user resource creation output.

Verify Users
------------
Log into the Test Kitchen instance.

```
kitchen login default-centos65
getent passwd alice
getent passwd bob
```

Encrypted Data Bags
-------------------
Encrypted with a shared key to store sensitive iformation on Chef Server. Node attributes are plain text and searchable, but cannot be shared between nodes. They are not secure!

When a data bag is created with `knife data bag create`, a file containing a shared key can be passed on the command line. This is used as the password to encrypt the data bag item contents.

When a node wants to decrypt the data bag item, it must pass the shared key on any `knife data bag` operations.

```
cd ./chef-playground
# Start chef-zero in a separate tab.
```

Generate a password to be used as the shared key.

```
openssl rand -base64 512 | tr -d '\r\n' > encrypted_data_bag_secret
```

This generates a 512 byte random string, converts binary to a base 64 string, and removes linefeeds (ensures the key will be the same in any platform).

Create a directory to hold sensitive data (say, API keys), and create a data bag item.

```
mkdir -p ./data_bags/api_keys
```

***api_keys/payment.json***
```
{
  "id": "credit_card",
  "api_key": "abc123-def-456-ghi789"
}
```

Finally, create the data bag in Chef Zero and add the item, ensuring it is encrypted.

```
knife data bag create api_keys
knife data bag from file api_keys payment.json --secret-file encrypted_data_bag_secret data_bag[api_keys::payment]
```

When using knife to show the data bag, you will get an encrypted result when the secret key is not passed.

```
knife data bag show api_keys credit_card
```

The id will remain in plaintext as Chef uses this field as the index. When encrypting data bags, you will lost the ability to search for data in the bag.

```
knife data bag show api_keys credit_card --secret-file encrypted_data_bag_secret
```

How does your node get the secret key?
- Do not store keys on the same system in which data resides.
- Use chef-vault!

chef-vault
----------
Reuses the public/private key pairs Chef uses for nodes to create a key encapsulation scheme. When a data bag item is created, a shared key is generated on the node.
- For each node that has access, the shared key is encrypted with the node's public key.
- This is stored on Chef Server.

A legitimate client key for your node must be registered with Chef Server.

```
knife client list
# Right now, this should output chef-validator and chef-webui.
# Chef Zero does not know it can store data on other nodes.
```

Generate a public/private keypair for your development workstation, whch was called devhost.
- Chef Zero won't check if we regenerate `./chef-playground/.chef/devhost.pem`.
- When we regenerate the client key, it ensures the matching public key is stored on Chef Server.

```
knife client create devhost --admin --disable-editing --file .chef/devhost.pem
knife client list
```

The `--admin` option allows the client to run APIs on nodes beyond itself. By default, `knife client create` opens an editor to modify the client configuration, which can be disabled with the `--disable-editing` flag.

You will need to associate the node with the client key.

```
knife node create devhost --disable-editing
knife node list
```

This is similar to the bootstrap process. A client key is generated and regitered with Chef Server.

Now we can create a data bag for storing passwords. Before doing so, you may need to install the chef-vault gem.

```
sudo gem install chef-vault
mkdir -p data_bags/passwords
```

***passwords/mysql_root.json***
```
{
  "id": "mysql_root",
  "password": "p4ssw0rd"
}
```

chef-vault installs a knife plugin to manage encrypted data bags.

```
knife vault create passwords mysql_root --json data_bags/passwords/mysql_root.json --search "*:*" --admins "admin" --mode client
```

You must specify users or nodes that have valid client keys using the `--search` and `--admins` parameters. In this example, we use both since we did not set up a valid client key for the admin user. When using Chef Server, you must use the option `--mode client`.

chef-vault can encrypt data only if Chef Server has valid client keys.

```
knife data bag show passwords mysql_root
# Will be encrypted.
knife vault show passwords mysql_root --mode client
# Will not be encrypted.
```

Roles
=====
Classify different types of services in your infrastructure.
- Load Balancer
- App Server
- DB Cache
- Monitoring

You should not add recipes directly to a nodes run list. Roles encapsulate the run lists and attributes required for a server to be what you need it to be. Roles allow you to configure many nodes identically without repeating yourself.

*Base Role:* A role containing all recipes that should run on every node.

Create a Web Server Role
------------------------
Roles are created and managed in the same fashion as data bags.

Copy `./chef-playground` from the previous section. And start chef-zero in a separate terminal window. Then, upload the fake node data to Chef Zero.

```
cd chef-playground
knife upload nodes
mkdir roles
cd roles
```

***roles/webserver.json***
```
{
  "name": "webserver",
  "description": "Web Server",
  "json_class": "Chef::Role",
  "chef_type": "role",
  "run_list": [
    "recipe[motd]",
    "recipe[apache]",
    "recipe[users]"
  ]
}
```

Upload and view the role on Chef Zero.

```
knife role from file webserver.json
knife role show webserver
knife node run_list set atwood "role[webserver]"
```

During a Chef run, the webserver role will be expanded into the entries of the role's run list.

Attributes and Roles
--------------------
Roles can contain attributes as well.

Create a base role with the recommended Chef recipes, as well as an attribute to tell `chef-client::default` to set the `init_style` to `runit`.

***roles/base.json***
```
{
  "name": "base",
  "description": "Common recipes for all nodes.",
  "json_class": "Chef::Role",
  "chef_type": "role",
  "run_list": [
    "recipe[chef-client::delete_validation]",
    "recipe[chef-client]"
  ],
  "default_attributes": {
    "chef_client": {
      "init_style": "runit"
    }
  }
}
```

Create the role in Chef Zero.

```
knife role from file base.json
knife role show base
```

It is recommended to only assign attributes to the default priorty in roles.

| Priority | Type                      |
| -------- | ------------------------- |
| Highest  | Automatic (ohai)          |
|          | Defined in Role           |
|          | Defined in Recipe         |
| Lowest   | Defined in Attribute File |

Roles and Search
----------------
Roles can be search items as well. Make sure to escape characters when needed.

```
knife search role "run_list:recipe\[apache\]"
```

Because recipes in a run list can be expanded, there are two ways to search for recipes in a node's run list.

```
knife search node "recipe:<name>"
# Does NOT include the expanded set of recipes within roles.
knife search node "recipes:<name>"
# Does include the expanded set of recipes within roles.
```

Similar search commands exist for roles and their expansion.

```
knife search node "role:<name>"
# Does NOT include the expanded set of role references.
knife search node "roles:<name>"
# Does include the expanded set of role references.
```

Role Cookbook
-------------
When a change is made to a role, it is reflected immediately across the infrastructure. Roles are not versioned in any way. This usually has the most impact with run lists.

Because cookbooks are versioned, a pattern has emerged of using a role cookbook in lieu of the run list feature of roles. Roles are still used for common attributes. The role run list is moved to a cookbook which makes use of the `include_recipe "<name>"`.

Regardless of this, roles are extremely useful for classification of resources and sharing of attributes across similar nodes.

```
knife search node "role:webserver"
```

Currently Chef Zero does not index roles for searching, so the above example will not work in this test.

Environments
============
Used to model server configurations required during each phase of your SDLC.
- Development
- Staging
- Testing
- Production

Every Chef Server starts with a `_default` environment.

Environments include attributes necessary for configuring your infrastructure.

Environments can contain version constraints, unlike roles.

Create a Dev Environment
------------------------
Environments can be created and managed in the same fashion as data bags and roles.

Copy the chef-playground directory from the previous secions, and start Chef Zero in a separate terminal tab.

Create an additional environments directory in `chef-playground/`.

```
mkdir environments
```

Environments can have one or more cookbook constraints as well. You can "pin" cookbooks to particular versions in an environment.

***environments/dev.json***
```
{
  "name": "dev",
  "description": "For developing!",
  "cookbook_versions": {
    "apache": "= 0.2.0"
  },
  "json_class": "Chef::Environment",
  "chef_type": "environment"
}
```

```
knife environment from file dev.json
knife environment show dev
```

Attributes and Environments
---------------------------
You can also provide environment-level attributes.

***environments/production.json***
```
{
  "name": "production",
  "description": "For prod!",
  "cookbook_versions": {
    "apache": "= 0.1.0"
  },
  "json_class": "Chef::Environment",
  "chef_type": "environment",
  "override_attributes": {
    "motd": {
      "message": "A production message."
    }
  }
}
```

```
knife environment from file production.json
knife environment show production
```

Environment attributes now have a place in the attribute precedence history.

| Priority | Type                      |
| -------- | ------------------------- |
| Highest  | Automatic (ohai)          |
|          | Defined in Environment    |
|          | Defined in Role           |
|          | Defined in Recipe         |
| Lowest   | Defined in Attribute File |

In this scenario, it may make sense to use override_attributes instead of default_attributes for environment attributes that need to take precedence over role or automatic attributes.

Putting all the Pieces Together
-------------------------------
Create a chef-zero directory.

```
mkdir chef-zero
cd chef-zero
```

Create an environments directory to contain environment definitions.

```
mkdir chef-zero/environments
cd chef-zero/environments
```

Copy `production.json` from the previous example.

We will also use roles in this example.

```
mkdir chef-zero/roles
cd chef-zero/roles
```

***roles/webserver.json***
```
{
  "name": "webserver",
  "description": "Web Server",
  "json_class": "Chef::Role",
  "chef_type": "role",
  "default_attributes": {
    "apache": {
      "port": 80
    }
  },
  "run_list": [
    "recipe[apache]"
  ]
}
```

Create a cookbooks directory.

```
mkdir chef-zero/cookbooks
cd chef-zero/cookbooks
```

Generate an apache cookbook.

```
chef generate cookbook apache
cd apache
```

Create your `.kitche.yml` file.

***apache/.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: chef_zero
  environments_path: ../../environments
  roles_path: ../../roles
platforms:
  - name: centos65
    driver:
      box: learningchef/centos65
      box_url: learningchef/centos65
suites:
  - name: prod
    provisioner:
      client_rb: # Modifies client.rb on the instance.
        environment: production
    driver:
      network:
      - ["private_network", {ip: "192.168.33.15"}]
    run_list:
      - role[webserver]
    attributes:
```

Normally, you would use the `chef-client::config` recipe to change settings in `/etc/chef/client.rb`.

```
node.default['chef_client']['config']['envrionment'] = 'production'
```

In this example, the private IP is set in the `suites:` stanza. Setting a value in the `provisioner:` stanza causes all items in the `suites:` stanza to inherit the value.

Create default attributes for your apache cookbook, making sure the values are different than what is set in the role.

***attributes/default.rb***
```
default['apache']['document_root'] = '/var/www/html'
default['apache']['port'] = 3333
default['motd']['message'] = 'Default message.'
```

***recipes/default.rb***
```
package 'httpd'

service 'httpd' do
  action [ :enable, :start ]
end

template '/etc/httpd/conf.d/custom.conf' do
  source 'custom.conf.erb'
  mode '0644'
  variables(
      :document_root => node['apache']['document_root'],
      :port => node['apache']['port']
  )
  notifies :restart, 'service[httpd]'
end

document_root = node['apache']['document_root']

directory document_root do
  mode '0755'
  recursive true
end

template "#{document_root}/index.html" do
  source 'index.html.erb'
  mode '0644'
  variables(
      :message => node['motd']['message'],
      :port => node['apache']['port']
  )
end
```

Generate the `index.html.erb` template.

```
chef generate template index.html
```

***templates/index.html.erb***
```
<html>
  <body>
    <h1><%= @message %></h1>
    <%= node["ipaddress"] %>:<%= @port %>
  </body>
</html>
```

Generate the `custom.conf.erb` template.

```
chef generate template custom.conf
```

***templates/custom.conf.erb***
```
<% if @port != 80 -%> # The '-' omits the line from the template.
  Listen <%= @port %>
<% end -%>

<VirtualHost *:<%= @port %>>
  ServerAdmin webmaster@localhost
  DocumentRoot <%= @document_root %>

  <Directory />
    Options FollowSymLinks
    AllowOverride None
  </Directory>

  <Directory <%= @document_root %>>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride None
    Order allow,deny
    allow from all
  </Directory>
</VirtualHost>
```

After running `kitchen converge`, your sit should be visible at the private IP on port 80.

Simulate a Development Environment
----------------------------------
We don't want new cookbook development to interfere with the stable 0.1.0 version of the production cookbook. First, increment the cookbook version to 0.2.0 in `metadata.rb`.

If you run `kitchen converge`, you will get an error stating version constraints could not be satisfied.

***environments/dev.json***
```
{
  "name": "dev",
  "description": "For development!",
  "cookbook_versions": {
    "apache": "= 0.2.0"
  },
  "json_class": "Chef::Environment",
  "chef_type": "environment",
  "override_attributes": {
    "apache": {
      "port": 8080
    },
    "motd": {
      "message": "Let's cook."
    }
  }
}
```

Add an additional development suite to `.kitchen.yml`.

***.kitchen.yml***
```
suites:
  # ...
  - name: dev
    provisioner:
      client_rb:
        environment: dev
    driver:
      network:
        - ["private_network", {ip: "192.168.33.16"}]
    run_list:
      - role[webserver]
    attributes:
```

Verify the new Test Kitchen instance is configured correctly, then converge.

```
kitchen list
kitchen converge dev-centos65
```

You should now be able to access the development site on port 8080. The port setting in the environment overrides the default attribute set in the apache cookbook, as well as the role.

Testing
=======
Automation of testing is critical!

Testing Rationale
-----------------
Test Kitchen gives you confidence that, if you deployed the same Chef code to a production environment, it would behave in the same manner. Just as it is best to introduce changes to application code in small batches, its best to do the same with infrastructure changes.

1. Code right the first time, because it costs more to fix later.
2. Find bugs and issues as early as possible.
3. Make changes in small batches, testing in between.

Chef provides mutliple testing tools designed to give feedback on issues with your code at the earliet possible time during the cookbook authoring process.

*Foodcritic:* Analyzes your Chef coding style.

*ChefSpec:* Helps you document and organize your code.

*Serverspec:* Verifies the cookbook behaves as expected.

Revisiting the Apache Cookbook
------------------------------
We will be adding Serverspec tests to the apache cookbook.

```
chef generate cookbook apache-test
cd apache-test
```

***.kitchen.yml***
```
driver:
  name: vagrant
provisioner:
  name: chef_zero
platforms:
  - name: centos65
    driver:
      box: learningchef/centos65
      box_url: learningchef/centos65
      network:
      - ["private_network", {ip: "192.168.33.38"}]
suites:
  - name: default
    run_list:
      - recipe[apache-test::default]
    attributes:
```

Verify there are no errors with `kitchen converge`, then create a default recipe.

***recipes/default.rb***
```
package 'httpd'

service 'httpd' do
  action [ :enable, :assist ]
end

template '/var/www/html/index.html' do
  source 'index.html.erb'
  mode '0644'
end
```

Generate an `index.html.erb` template.

```
chef generate template index.html
```

Recall that the ERB template processor looks for `<%= %>` tags for evaluation. If the tag does not have an equals sign, it is instead evaluated as a scriptlet.

```
<% for @interface in node['network']['interfaces'].keys %>
  * <%= @interface %>
<% end %>
```

This evaluates as a for loop iterating over keys in `node['network']['interfaces']`.

***templates/index.html.erb***
```
<html>
  <body>
    <pre><code>
      This site was set up by <%= node['hostname'] %>
      Network addresses are:
      <% node['network']['interfaces'].keys.each do |iface_name| %>
        * <%= iface_name %>: <%= node['network']['interfaces'][iface_name]['addreses'].keys[1] %>
      <% end %>
    </code></pre>
  </body>
</html>
```

Lastly, run `kitchen converge` to apply changes.

Test Automation with Serverspec
-------------------------------
By default, Test Kitchen will look in `test/integration` for test related files. There needs to be a directory under `test/integration` that matches the suite name.

```
mkdir -p test/integration/default
```

Additionally, we create another directory to tell Test Kitchen to use the serverspec plugin.

```
mkdir test/integration/default/serverspec
```

By convention, Serverspec expects files containing test code to end in the suffix `*spec.rb`.

***serverspec/default_spec.rb***
```
require 'serverspec'

set :backend, :exec

describe 'web site' do
  it 'responds on port 80' do
    expect(port 80).to be_listening 'tcp'
  end
end
```

The require statement is used to load a gem library so that Serverspec classes and methods can be referenced. The set statement lets us configure how Serverspec runs.
- Setting the `:backend` property to `:exec` tells serverspec that the test code will be running on the same machine where it is being evaluated.

Tests are written in the RSpec DSL using `describe` and `it` statements. In this case, the test checks if our website is listening to port 80 on TCP.

```
kitchen setup
kitchen verify
```

When `kitchen setup` runs, it inspects the directory structure in `/test/integration/<suite>/<plugin>`.
- Test Kitchen will load any plugins required for testing in your sandbox instance as indicated by the plugin directory name.
- In this case, Test Kitchen makes sure the test node has the necessary libraries and gems for running Serverspec tests.

RSpec DSL Syntax
----------------
Uses a describe block to contain a group of tests.

```
describe 'entity' do
  <tests>
end
```

Group tests in a meaningfull manner and describe the entity or thing being tested. The entity string serves as documentation for humans on the resource tested.

Actual tests are contained within an `it` block inside the describe statement.

```
describe 'entity' do
  it 'description'
    '<test>'
  end
end
```

The description describes the specific test to be performed. Tests themselves should be written in expect form.

```
describe 'entity' do
  if 'description'
    expect(resource).to matcher matcher_parameter
  end
end
```

A resource, also called a subject or command, is the first argument of an expect block.
- It expresses the thing to be tested.
- Serverspec and ChefSpec supply custom resource class implementations to perform a variety of checks.

A matcher is used to define positive or negative expectations on a resource.
- Positive via `expect(...).to`.
- Negative via `expect(...).not_to`.

```
describe 'web site' do
  it 'responds on port 80' do
    expect(port 80).to be_listening 'tcp'
  end
end
```

Serverspec documents all included resources and matches on http://serverspec.org. The latest standard is RSpec 3.0.

More Serverspec Resources
-------------------------
Common code can be moved to `spec_helper.rb`.

***serverspec/spec_helper.rb***
```
require 'serverspec'
set :backend, :exec
```

This can be referenced in any `*spec.rb` files.

***serverspec/default_spec.rb***
```
require 'spec_helper'

describe 'web site' do
  it 'responds on port 80' do
    expect(port 80).to be_listening 'tcp'
  end
end
```

Start your Test Kitchen instance and run `kitchen verify`. There should be no changes in the tests. Lets add a test that verifies the content of our webpage is correct.

```
it 'returns eth1 in HTML body' do
  expect(command('curl localhost:80').stdout).to match /eth1/
end
```

This test has the added benefit of checking if the vagrant box has eth1 enabled.

```
kitchen verify
```

Many Serverspec resources require Serverspec to detect information about the test node OS.
- The package resource for example.

```
describe 'httpd' do
  it 'should be installed' do
    expect(package 'httpd').to be_installed
  end
end
```

By default, Serverspec will try to detect the OS. On some platforms you will need to override this with the `set` method. For Windows, you will need to include the following in your `*spec.rb` files.

```
set :os, :family => 'windows'
```

For ease for use, `kitchen test` performs the following in order:
1. `kitchen destroy`
2. `kitchen create`
3. `kitchen converge`
4. `kitchen setup`
5. `kitchen verify`
6. `kitchen destroy`

Test Automation with Foodcritic
-------------------------------
The drawback to Serverspec is it requires a full converge for every test. Foodcritic is designed to provide feedback as you edit, and can be integrated with editors. Foodcritic performs checks against, your code, called rules.

All rules are documented at acrmp.github.io/foodcritic/#.

```
cd ./apache-test/
foodcritic .
```

Each "broken" rule will be displayed in console output. As an example, FC008 indicates that the `metadata.rb` maintainer and maintainer_email fields should be modified from the default.

Foodcritic can be integrated with various build tools. Additionally, it can be extended with custom rules. This is useful for catching bugs before deploying to test environments.

Test Automation with ChefSpec
-----------------------------
ChefSpec catches errors before development, and produces "runnable" documentation.

ChefSpec uses the RSpec description form to create runnable documentation.

```
describe '<recipe_name>' do
  <perform in-memory Chef run>
  <examples here>
end
```

Uses classes and methods from the chefspec gem, and utilizes an `expect` form similar to serverspec.

Runnable documentation means that ChefSpec does not perform any actions.
- Performs an in-memory Chef run to verify cookbook syntax.

Example: Check to verify your default recipe installs httpd.

***spec/default_spec.rb***
```
require 'chefspec'

describe 'apache-test::default' do
  chef_run = ChefSpec::Runner.new.converge('apache-test::default')

  it 'installs apache2' do
    expect(chef_run).to install_package('httpd')
  end
end
```

Write Your First ChefSpec Test
------------------------------
```
cd ./apache-test
mkdir spec
```

***spec/default_spec.rb***
```
require 'chefspec'

describe 'apache-test::default' do
  chef_run = ChefSpec::Runner.new.converge('apache-test::default')

  it 'installs apache2' do
    expect(chef_run).to install_package('httpd')
  end
end
```

Does not require any special ChefSpec commands to run.

```
cd ./apache-test
chef exec rspec --color
```

Lazy Evaluation with Let
------------------------
ChefSpec commonly uses the let helper to cache the results of the `ChefSpec::Runner`.

```
require 'chefspec'

describe 'apache-test::default' do
  let(:chef_run) { ChefSpec::Runner.new.converge(described_recipe) }

  it 'installs apache2' do
    expect(chef_run).to install_package('httpd')
  end
end
```

`let()` delays evaluation of the ChefSpec::Runner until when it is first used instead of when it is referenced in the source code.
- Allows RSpec to cache the results of `ChefSpec::Runner` when it is used multiple times in the same example.
- Allows you to specify the recipe under test only once in your `describe` block as documentation.

Generate a Coverage Report
--------------------------
`ChefSpec::Converge.report!` will generate a list of resources that have corresponding examples as documentation.

The exclamation point indicates the method is dangerous.
- In this case, the coverage report must run after all tests are complete and not more than once in a program.

***spec/default_spec.rb***
```
require 'chefspec'

at_exit {ChefSpec::Coverage.report!}

describe 'apache-test::default' do
  let(:chef_run) { ChefSpec::Runner.new.converge(described_recipe) }

  it 'installs apache2' do
    expect(chef_run).to install_package('httpd')
  end
end
```

Run `chef exec rspec --color` and note the report on the number of resources in your code and how many have been tested in your specs.

Share Test Code with spec_helper.rb
-----------------------------------
ChefSpec supports moving common code used in tests to `spec_helper.rb`, much like Serverspec.

***spec_helper.rb***
```
require 'chefspec'

at_exit {ChefSpec::Coverage.report!}
```

***default_spec.rb***
```
require 'spec_helper'

describe 'apache-test::default' do
  let(:chef_run) { ChefSpec::Runner.new.converge(described_recipe) }

  it 'installs apache2' do
    expect(chef_run).to install_package('httpd')
  end
end
```

When you run `chef exec rspec --color`, there should be no difference in output.
