អត្ថបទស្ដីអំពីVagrant​ - Vagrant​+Chef
===================================

មាតិការ
=========

* [១. Vagrant​ provisioning](#provisioning)
* [២. Install PHP](#installPHP)
* [៣. Chef](#chef)
  * [៣.១. ហេតុអ្វីយើងប្រើChef?](#why)
  * [៣.២. កំនត់និយមន័យនៅក្នុងchef](#why)
* [៤. Vagrant + Chef](#vagrantChef)
  * [៤.១. config Vagrantfile](#conf)
  * [៤.២. Add cookbooks](#cookbooks)
  * [៤.៣. Reload provision](#reload)
* [៥. Chef provision vs shell provision](#comp)
* [៦. Recap](#recap)
* [៧​.References](#ref)

--------------------------------------------------------------

## <a name="provsioning">១. Vagrant​ provisioning</a>

នៅក្នុងអត្ថបទមុន យើងបានស្វែងយល់មូលដ្ឋានខ្លះៗអំពីvagrant ដែលអាចបង្កើត1 VM បំរើការអភិវឌ្ឍwebយ៉ាងងាយជាមួយubunu 12.04 និងshell provisioning។

ជាដំបូងយើងមើលឡើងវិញ2 file​​ config សំខាន់គឺ Vagrantfile & setup.sh

Vagrantfile
```
Vagrant.configure(2) do |config|
    config.vm.box = "hashicorp/precise32"
```

setup.sh
```
## <a name="installPHP">២. Install PHP</a>

sudo apt-get install -y python-software-properties build-essential
sudo add-apt-repository -y ppa:ondrej/php5
sudo apt-get update
sudo apt-get install -y git-core subversion curl php5-cli php5-curl php5-mcrypt php5-gd
```

មានបីគំនិតសំខាន់ដែលត្រូវពិភាក្សានៅទីនេះជាមួយprovisioning ដែលសរសេរដោយ shell script
 * ទីមួយ គឺ VagrantសរសេរដោយRuby ចំនែកឯ file setup.sh ជាshell script។ ចំនោទចោទសួរថា តើមានរបៀបណាដែលប្រើតែភាសាតែមួយប្រភេទ? នេះមិនមែនជាការចាំបាច់ទេ
 តែវាច្បាស់ណស់provisioning ក៏មានstructureដូច Vagrantfileដែរ ហើយនឹងសមស្របបំផុត។
 * ទីពីរ គឺអាចមើលឃើញថា ការដំឡើង php ស្មុកស្មាញ ហើយមានmoduleច្រើន តែមិនប្រាកដថាវាគ្រប់គ្រាន់នោះឡើយ។ ព្រោះវាផ្អែកទីលើបទពីសោធន៍អ្នកសរសេរfile setup។
 * ទីបី គឺPHP វាដំណើរការតែលើUbuntu ចំនែកឯCentOS, Fedora វាមិនដំណើរការនោះទេ(? ) ដូច្នេះ ប្រសិនបើ spec ផ្លាស់ប្ដូរ ហើយvagrant boxមិនមែនជាUbunuទៀតនោះទេ
 យើងនឹងត្រូវសរសេរfile setup.shឡើងវិញ? គឺវាមិនខុសពីបង្កើតprojectថ្មីនោះទេ។
 
ខាងលើនេះ ជា៣ចំនោទសួរ ដែលយើងអាចឃើញបានតារយៈ shell provisioning។ នោះមិនមែនជាបញ្ហាចម្បងនោះទេ តែអាចមាន១VM គ្រប់គ្រងយ៉ាងល្អ support provisioning
ល្អ គឺshell មិនមែនជាជម្រើសល្អបំផុតនោះឡើយ។ សំណាងដែរដែលយើងមាន provisioning។

មាន៣ឈ្មោះដែលបានលើកឡើងនៅក្នុងdocumentរបស់Vagrant នោះគឺ Ansible, Chef & Puppet។ អត្ថបទនេះនឹងបង្ហាញអំពី `Chef`។ មិនមែនមកពីChefមានអ្វីដែលសាមញ្ញ
ជាង២ផ្សេងទៀតនោះទេ និយាយរួមទៅឈ្មោះរបស់វាក៏មានភាពទាក់ទាញផងដែរ។

## <a name="chef">៣. Chef</a>


chef ជាsoftwareគ្រប់គ្រងconfigអោយserver។ បណ្ដារconfig របស់server អាចយល់បានថាជា "recipes" ដែលត្រូវបានសរសេរដោយRuby និង Domain-specific-language (DSL)។
Chefមិនត្រឹមតែមានសមត្ថភាពគ្រប់គ្រង់ការបង្កើតម៉ាស៊ីនមួយនោះទេ ថែមទាំងគ្រប់systemទាំងមូលរបស់ក្រុមហ៊ុនផងដែរ។ Chefអាចរួមជាមួយplatform Cloud ដូចជា​ Rackspace,
Internap, Amazone EC2, Google Cloud Platform, OpenStack, SoftLayer & Microsoft Azure​ ដើម្បីអាចconfigដោយស្វ័យប្រវត្តិនូវserverជាច្រើន
ក្នុងពេលតែមួយ។ ស្វ័យប្រវត្តិ គឺជាចំនុចដែលធ្វើអោយChefមានភាពលេចធ្លោឡើង។

### <a name="why">៣.១. ហេតុអ្វីយើងប្រើChef?</a>


  * ប្រសិទ្ធិភាព: Configរបស់serverទាំងអស់នឹងរក្សានៅrepository​ តែមួយគត់។
  * លទ្ធភាពពង្រីកបន្ថែម: អ្នកគ្រាន់តែបង្កើនតួលេខserverរបស់អ្នកឡើង បែងចែកពួកគេតាម role & node។ chef​ នឹងរាប់រងផ្នែកដែលនៅសល់។
  * ងាយស្រួល នឹងសន្សសន្ចៃពេលវេលា: អ្នកមិនចាំបាច់ធ្វើចុះឡើង រាប់ដប់ដង ក្នុងការដំឡើងsoftwareនោះទេ អ្នកគ្រាន់តែបង្កើត1 node chef ចំនែកផ្នែកដែលសេសសល់នឹងដំឡើងដោយ
  ស្វ័យប្រវត្តិ។
  * ឯកសារ: ក៏ដូចជាclean codeដែរ  គឺឯកសារច្បាស់លាស់បំផុតសំរាប់projectរបស់អ្នក។ chefជាឯកសាររបស់server systemរបស់អ្នក រួមទាំងពត័មានអំពីserverដែលបានរក្សា
  នៅក្នុង chef recipes។(ច្បាស់ណាស់អ្នកមិនអាចនិយាយថាshell scriptជាឯកសាររសប់អ្នកនោះទេ។)
  
### <a name="def">៣.២. កំនត់និយមន័យនៅក្នុងchef</a>

- Chef Client: command line tool ប្រើប្រាស់ក្នុងការconfig server
- Chef Solo: ជំនាន់របស់Chef Client, config serverដែលមិនឋិតក្នុងserverណាផ្សេង។
- Node: Hostដំណើរការ Chef Clientអាចជាweb server, database server ឫ  ប្រភេទserverណាសមួយផ្សេងទៀត។​ 
- Recipes: ១ file Ruby ក្នុងនោះមានផ្ទុកcommandsដំណើរការនៅលើnode (nginx ssl module, apache php module)។
- Resources: Resourcesរបស់node ផ្ទុកបណ្ដារfile, directories, users & services។
- Cookbook: សំណុំបណ្ដារ Recipes(nginx cookbook, postgresql cookbook)។
- Role: Configអាចប្រើប្រាស់បានnodeជាច្រើន(web role, database role,..)។
- Template: 1 file ផ្ទុកនូវ attributes,ប្រើដើម្បីបង្កើតfile config (នេះជា 1 file erb)។
- Attribute: អញ្ញាតត្រូវបានបញ្ជូនតាមរយៈChef និងបានប្រើប្រាស់ក្នុងrecipes, templates។

## <a name="vagrantChef">៤. Vagrant + Chef</a>

មិត្តអានគួមានVMមួយ ដើម្បីអ្នកអាចធ្វើតាមការណែនាំ ដោយមិនខ្លាចខូចម៉ាស៊ីនរបស់អ្នក។

ខាងក្រោមនេះជាការណែនាំជាមួយ provisioning​​ អោយvagrant ប្រើប្រាស់Chef។

### <a name="conf">៤.១. config Vagrantfile</a>


ដំបូងអ្នកedit file Vagrantfile ដែលអនុញ្ញាតធ្វើ provisioningដោយchef មិនធ្វើដោយshellដូចមុនទេ។
```
    Vagrant.configure("2") do |config|
        config.vm.provision "chef_solo" do |chef|
            chef.add_recipe "apache"
        end
    end
```

Options របស់Chef Solo
```
- **cookbooks_path:** (string or array) ចង្អុលបង្ហាញពីកន្លែងសក្សាcookbooks.
- **data_bags_path:** (string) កន្លែងរក្សាdata bag។ ជាទូទៅគឺមិនមានdata bag ណាឡើយ។
- **environments_path:** (string) កំណត់មជ្ឈដ្ឋាន។ ជាទូទៅdirectoryនេះមិនបានបង្កើតទេ។
- **environment:** (string) មជ្ឈដ្ឋានដែលអ្នកចង់Chefដំណើរការ ហើយជា១ផ្នែករបស់វា។
- **recipe_url:** (string) URL ដែលមាន cookbooks
- **roles_path:** បញ្ជីបណ្ដារ paths ដើម្បីកំណត់routes។ ជាទូទៅបញ្ជីនេះទទេ។
- **synced_folder_type:** មធ្យោបាយដើម្បីsync folder, ធានាថាprovisioning ប្រព្រឺត្តទៅល្អ។
```

### <a name="cookbooks">៤.២. Add cookbooks</a>


បន្តមកទៀតបង្កើតdirectoryអោយChef cookbook។ ពីroot directoryរបស់app អ្នកបង្កើតdirectoryមួយ ក្រោយមកadd recipesតាមដ្យាក្រាមខាងក្រោម។
```
    dinhhoanglong@long-inspiron:~/study/vagrant-sample$ tree
    .
    ├── cookbooks
    │   └── apache
    │       └── recipes
    │           └── default.rb
    ├── README.md
    ├── Vagrantfile
    └── web
        ├── index.html
        └── phpinfo.php
```

```
Recipes và cookbooks អាចជាសញ្ញាណមួយថ្មីសំរាប់អ្នកទើបនឹងចាប់ផ្ដើមVagragtជាមួយChef។ តែអ្វីដែលអ្នកត្រូវដឹងនោះ វាស្ទើតែជានិយមន័យ
ដោយវាមានcookbooksជាច្រើនរបស់ Chef ដែលបានចែករំលែកនៅលើGithub។ អ្នកក៏អាចស្វែងយល់ និងប្រើប្រាស់វានៅក្នុងprojectរបស់អ្នក។ Check "super market" ដើម្បីស្វែងរក
cookbookដែលអ្នកត្រូវការ។
```

> https://supermarket.chef.io/

ដើម្បីប្រើប្រាស់បណ្ដារshared chef cookbook ក្រៅពីdownloadដោយដៃរបស់អ្នក។ អ្នកអាចប្រើtoolដែលមានស្រាប់របស់chef។ របៀបផ្សេងមួយទៀត គឺកប្រើប្រាស់ git submodule
(កាន់តែងាយស្រួលជាមួយGit)។

ជាមួយ Knife
```
knife cookbook site install apache2
```

```
git submodule add https://github.com/opscode-cookbooks/apache2.git cookbooks/apache2
```

ប្រសិនបើប្រើប្រាស់ knife អ្នកអាចdownload cookbookចេញពីdirectoryនៃprojectផ្សេង ដើម្បីយកមកប្រើប្រាស់ក្នុងករណីផ្សេទៀត។ ចំពោះជាំមួយ git submodule គឺrepositoryរបស់អ្នកមិនកើនឡើង ហើយគ្រប់គ្រង
ទាំងមូលយ៉ាងងាយស្រួល។

### <a name="reload">៤.៣. Reload provision</a>

អ្នកអាចload provisionដោយcommandខាងក្រោម
```
vagrant provision
```

## <a name="comp">៥. Chef provision vs shell provision</a>

មកដល់ពេលនេះអ្នកបានយល់អំពីពីការប្រើប្រាស់ Chef។ ហើយអ្នកអាចឆ្លើយនឹងសំនួរខាងលើដែលបានលើកឡើងផងដែរ។

- ភាពខុសគ្នារវាង shell និង ruby: Recipe & file config ផ្សេងទៀត សុទ្ធតែសរសេរដោយRuby។ ដូច្នេះយើងនឹងអាចប្រើប្រាស់តែមួយភាសាប៉ុណ្ណោះ អោយprojectនោះ។ លើសពីនេះទៅទៀត Rubyងាយស្រួលអានជាង បើធៀបនឹងshell script។
- មានការsettingគ្រប់គ្រាន់ឫនៅ: ដោយប្រើប្រាស់cookbook ដែលបានចែករំលែកនៅលើinternet និងជាពិសេសទៅទៀតគគឺក supermarket របស់chef យើងអាចស្ងប់ចិត្តថា មជ្ឈដ្ឋានធ្វើការបាន config ដោយអ្នកដែលមានបទពិសោធន៍។
- ធ្វើការជាមួយOSផ្សេងគ្នា: សូមមើលរបៀបដែល php cookbookដំឡើងmemcacheអោយphp។

> https://github.com/opscode-cookbooks/php/blob/master/recipes/module_memcache.rb

```
case node['platform_family']
    when 'rhel', 'fedora'
        %w{ zlib-devel }.each do |pkg|
            package pkg do
                action :install
            end
        end
        php_pear 'memcache' do
            action :install
            # directives(:shm_size => "128M", :enable_cli => 0)
        end
    when 'debian'
        package 'php5-memcache' do
            action :install
        end
    end
```


## <a nakme="recap">៦. Recap</a>

ខាងលើនេះ ជាការនិយាយអំពី Chef ក្នុងVagrant។ សង្ឃឹមថាមានអត្ថប្រយោជន៍ដល់អ្នក អាចអោយអ្ន​កយល់បន្ថែមអំពីtool ក៏ដូចជាក្នុងការsetup system អោយserver។

## <a name="ref">៧​.References</a>

[1] https://adamcod.es/2013/01/15/vagrant-is-easy-chef-is-hard-part2.html

[2] http://leopard.in.ua/2013/01/04/chef-solo-getting-started-part-1

[3] https://martinfowler.com/articles/vagrant-chef-rbenv.html
