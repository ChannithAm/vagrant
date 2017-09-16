អត្ថបទស្ដីអំពីVagrant
================

មាតិការ
===========

* [១. Vagrantជាអ្វី?](#intro)
* [២. ផលប្រយោជន៍របស់Vagrant](#pros)
  * [២.១. ចំពោះdeveloper](#dev)
  * [២.២. ចំពោះoperations engineer](#operation)
  * [២.៣. ចំពោះDesigner](#design)
* [៣. ការដំឡើង](#install)
* [៤. Project Setup](#setup)
* [៥. Boxes](#boxes)
* [៦. Up & SSH](#upSSH)
* [៧. Synced folder](#sync)
* [៨. Provisioning](#provisioning)
* [៩. Netwroking](#networking)
* [១០. Recap](#recap)
* [១១. References](#ref)


## <a name="intro">១. Vagrantជាអ្វី?</a>

* Vagrant ជាឧបករណ៍ដំឡើង និងគ្រប់គ្រងម៉ាស៊ីននិមិត្ត
អាចប្រើបាននៅលើប្រព័ន្ធប្រតិបត្តិការ Ubunu, MacOS
និងWindows។
* ម៉ាស៊ីននិមិត្តផ្ដល់ដោយបណ្ដារprovider ដូចជា Virtualbox,
VMware, AWS,...។ បណ្ដារsoftwareត្រូវបានដំឡើងដោយប្រើ
ប្រាស់**provisioner**បានក្លាយជាស្តង់ដាររួម(shell scripts,
Chef, Puppet)។
* Vagrant​ អាចគ្រប់គ្រងម៉ាស៊ីននិមិត្តច្រើប្រភេទខុសគ្នា
ហើយដំឡើងsoftwareអោយកុំព្យូទ័រដោយស្វ័យប្រវត្តិ ដោយមិន
គិតថាកំពុងប្រើប្រព័ន្ធប្រតិបត្តិការ(OS)ឫក៏distributionណា។

## <a name="pros">២. ផលប្រយោជន៍របស់Vagrant</a>

### <a name="dev">២.១. ចំពោះdeveloper</a>

Softwareទាំងអស់ ក៏ដូចជាការconfigផ្សេងទៀត ត្រូវបាន
អនុវត្តន៍ដោយអ្នកបង្កើត `vagrant` ចំណែកសមាជិកផ្សេងៗ នឹងនៅក្នុង
មជ្ឈដ្ឋានអភិវឌ្ឍតែមួយ។ មិនបែងចែកថា អ្នកប្រើប្រាស់ Ubunu, Mac, ឫWindowsនោះទេ
គឺអ្នកនឹងនៅក្នុងមជ្ឈដ្ឋានជាមួយសមាជិកផ្សេងទៀត សូម្បីតែproduction server។
ទាំងនេះបានកាត់បន្ថយបាននូវពេលវេលាដំឡើង កាត់បន្ថយbugនឹងកើត
ឡើងតែមជ្ឈដ្ឋានមួយជាក់លាក់តែប៉ុណ្ណោះ។ 

### <a name="operation">២.២. ចំពោះoperations engineer</a>

- អ្នកអាចsetupនៅពេលជាមួយគ្នា បានVM networkជាច្រើន យ៉ាងសាមញ្ញ៖
ប្រសិនគ្រាន់តែប្រើVirtualBoxម្យ៉ាង អ្នកត្រូវតែsetupជាបន្តបន្ទាប់
ដល់ម៉ាស៊ីនទាំងអស់នៅក្នុងប្រព័ន្ធserver។ តែចំពោះ vagrant
អ្នកគ្រាន់តែត្រូវការ 1 file config ហើយអ្នកអាចbuild/edit
ក្នុងពេលតែមួយ បានម៉ាស៊ីនជាច្រើនដោយងាយស្រួល។
- គ្រប់គ្រងsourceដោយងាយស្រួល៖ ដោយpushការsetupទៅមួយtextfile
និងគ្រប់គ្រងដោយsubversion control​(git ជាដើម) 
អ្នកនឹងមានភាពងាយស្រួលត្រួតពិនិត្យការbuilរបស់ខ្លួន។ 
ក្នុងករណីជួបនូវកំហុសអ្វីមួយ គឺមានភាពងាយស្រួលក្នុងការត្រួតពិនិត្យ(check)
ផ្លាស់ប្ដូរ ឫក៏revertមកសភាពដើមវិញដែលគ្មានកំហុស។
- ភាពសំបូរបែប​៖  ជាមួយប្រព័ន្ធ boxes ដ៏សំបូរបែប និងទូលំទូលាយ
អ្នកអាចសាកល្បងជាមួយប្រពន្ធប្រតិបត្តិការ(OS)ផ្សេងៗគ្នា ក៏ដូចជាdistributionខុសគ្នា។
ក្រៅពីនេះ ជាមួយនឹងការប្រើប្រាស់បណ្ដារ `provision tool` អ្នកងាយស្រួលក្នងការ
ដំឡើងsoftwareនៅលើអំបូរOSខុសៗគ្នា។

## <a name="design">២.៣. ចំពោះDesigner</a>

អ្នកមិនបន្តការលំបាកជាមួយនឹងការដំឡើងwebsitដំណើរការនៅលើ
ម៉ាស៊ីនរបស់អ្នក ជាមួយintructionរាប់សិបទៀតនោះទេ។ ហើយក៏មិនត្រូវាការ
ពិនិត្យមើលថាwebsiteបានដំណើរការល្អនៅលើម៉ាស៊ីនរបស់developerឫនៅនោះទេ
អ្វីដែលអ្នកត្រូវការនោះគឺ ដំឡើងvagrant ហើយvagrant up
នឹងជួយបង្ហើយកិច្ចការរបស់អ្នក។ ក្រោយមកអ្នកអាចត្រឡប់មកធ្វើកិច្ចការ
របស់អ្នក Desig!

## <a name="install">៣. ការដំឡើង</a>

Vagrant ត្រូវបានសរសេរដោយRuby ដែលជា១ repository ឋិតនៅលំដាប់top trending រសប់RubyនៅលើGithub។ អ្នកអាចមើលទៅលើប្រភព(source code)
លើទំព័រ  https://github.com/mitchellh/vagrant។ អ្នកមិនចាំបាច់ដំឡើងRubyនៅលើកុំព្យូទ័ររបស់អ្នកទេ ដោយគ្រាន់តែdownloadដោយផ្ទាល់នៅលើគេហទំពរ័ផ្លូវការ
http://www.vagrantup.com/downloads ។ ចំពោះការដំឡើងវិញរិតតែងាយស្រួលទៅទៀត តែដើម្បីប្រើប្រាស់វា ជាពិសេសmasterទៅលើវានោះ អ្នកត្រូវមានចំនេះដឹងអំពីRuby។

## <a name="setup">៤. Project Setup</a>

បន្ទាប់ពីរដំឡើងVagrant រួចរាល់អ្នកអាចដំណើរការ(run)ពីterminal ដោយវាយ `vagrant` ។ ដើម្បីបង្កើតproject ១ អ្នកអាចធ្វើដូចខាងក្រោម៖
```
mkdir vagrant_simple
cd vagrant_simple
vagrant init
```

បន្ទាប់ពីជំហ៊ាននេះ 1 file ឈ្មោះថា Vagrantfile នឹងត្រូវបានបង្កើតឡើងនៅក្នុងdirectory `vagrant_simple` ។ នៅក្នុងfileនេះមានកិច្ចការពីរដែលត្រូវបំពេញ៖ 
  * 1st ចង្អុលបង្ហាញពី root directory អោយprojectរសប់អ្នក។ រាល់ការseupថ្ងៃក្រោយនឹងត្រូវបានគិតទៅrelative path ពី Vagrantfile។
  * 2nd setting អោយprojectទាំងមូល រួមមាន OS, dirstribution, resources និងបណ្ដារsoftwareដែលបានដំឡើង ក៏ដូចជារបៀបaccessចូលទៅក្នុងម៉ាស៊ីន។

នេះជា 1 file ដែលបានសរសេរដោយRuby (ទោះបីជាកន្តុយរបស់វាមិនមែន .rb)។

## <a name="boxes">៥. Boxes</a>

ប្រសិនបើបានដំឡើង 1 OS ដូចរបៀបធម្មតា អ្នកនឹងត្រូវចំនាយពេលកន្លះម៉ោង ដោយគ្រាន់តែចុច (click) mouse និងចុច enter។ ជាមួយVagrant boxes អ្នកសន្សំសន្ចៃពេលវេលា
បានច្រើន។ Vagrant box ជាbase image ជួយអ្នកអោយងាយស្រួលក្នុងការបង្កើត 1 VM (virtual machine)។ ខាងក្រោមនេះជា commandដើម្បីclone box ពីhost machine។
```
vagrant box add hashcorp/precise32
```

Vagrantនឹងស្វែងរកboxដោយស្វ័យប្រវត្តិជាមួយនឹងឈ្មោះ `hashcorp/precise32` ពី  **HashiCorp's Atlas box catalog** នឹងdownload box នេះ
មកកុំព្យួទ័ររសប់អ្នក។ box​នេះអាចប្រើបានprojectជាច្រើនផងដែរ។ រៀងរាល់projectបានបង្កើតឡើងម្ដងៗអំពីboxនេះ vagrantនិងcopyដោយស្វ័យប្រវត្តិចេញជា​ 1 image
ផ្សេងទៀត។ រាល់ការកែ ឫក៏ផ្លាស់ប្ដូរ មិនមានឥទ្ធិពលដល់base boxនោះទេ។ ដើម្បីset box អោយprojectរបស់អ្នក អ្នកអាចedit file Vagrantfileដូចខាងក្រោម៖
```
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise32"
end
```

Box ត្រូវបានបង្កើតដោយ `user hashicorp` និងឈ្មោះbox: precise32។ ក្រោយពីដំឡើងboxរួចរាល់ អ្នកនឹង
មានvmមួយក្នងម៉ាស៊ីនរបស់អ្នក គឺជា ubunu 14.04 LTS 32 bit។ អ្នកក៏អាចចុះឈ្មោះaccount និង បង្កើត+share​ box របស់អ្នកអោយអ្នកដទៃប្រើប្រាស់ផងដែរនៅលើ 
https://atlas.hashicorp.com ។ 

## <a name="upSSH">៦. Up & SSH</a>

បន្ទាប់ពីដំឡើងរួចរាល់អ្នកអាច seupជាលើកដំបូង ដោយអ្នកបើក ហើយloginដូចខាងក្រោម៖
```
vagrant up
vagrant ssh
```

ជាមួយលើកដំបូងបើកកុំព្យូទ៏រ អ្នកត្រូវរងចាំដើម្បីvagrant clone ពី​ base box ចំនែកពេលលើកក្រោយ គឺវានឹងដំណើរកាលឿន ពេលបើកកុំព្យូទ័រ។ ក្រោយពេល login អ្នកមានសិទ្ធគ្រប់គ្រាន់ដើម្បី
ប្រើប្រាស់កុំព្យូទ័ររបស់អ្នក លើកលែងតែការលុបfile* ពិសេសៗ។

## <a name="sync">៧. Synced folder</a>

ទោះបីជាក្នុងដៃរបស់អ្នកមានម៉ាស៊ីននិមិត្តមួយបានបង្កើតឡើងយ៉ាងងាយស្រួលក៏ដោយ​ ប៉ុន្តែមិនមែននណាក៏ចង់សរសេរcodeផ្ទាល់នោះដែរ។ ជាdefaultគឺថាvagrantគាត់supportល្អក្នុងការ
synce​ file រវាងserver និងVM។ ហើយជាធម្មតា directory root របស់server (ដែលផ្ទុក Vagrantfile)នឹងត្រូវបានsysnceនៅ `vagrant/Vagrantfile` ក្នុងម៉ាស៊ីននិមិត្ត។
ចំពោះfile*ពិសេស ដែលខ្ញុំចង់និយាយ គឺជាfileនេះឯង។ ទោះបីជាអ្នកមានសិទ្ធគ្រប់គ្រាន់ធ្វើការជាមួយម៉ាស៊ីននិមិត្តក៏ដោយ តែដាច់ខាត់កុំលុបfile config របស់system។

ដើម្បីធ្វើតេស្តមុងារsynce អ្នកធ្វើដូចខាងក្រោម
```
vagrant@precise32:~$ cd /vagrant
vagrant@precise32:/vagrant$ echo "test sync" > test_sync.txt
vagrant@precise32:/vagrant$ exit
logout
Connection to 127.0.0.1 closed.
ubuntu@DinhHoangLong:~/study/vagrant/vagrant_sample$ cat test_sync.txt
test sync
```

Vagrant support protocolច្រើនក្នុងការ synce file ក្នុងdirectory share។ អ្នកអាចប្រើប្រាស់ smb, nfs, rsync. ពេលproviderជា VirtualBox, ជាdefaultអ្នក
នឹងប្រើប្រាស់ VirtualBox share folder។ តែអ្នកគួចំនាំដែរថា VirtualBoxមានកំហុសខ្លួនឯងត្រង់ចំនុចនេះ គឺជាប់ទាក់ទងដល់ `sendfile` ដូច្នេះហើយប្រសិនបើប្រើប្រាស់default
sync type ដើម្បីអោយប្រាកដនោះ អ្នកចាំបាច់config server ដូចខាងក្រោម

**Nginx**
```
sendfile off;
```

**Apache**
```
EnableSendfile off
```

ពត័មានលំអិត អ្នកអាចមើលនៅទីនេះ https://docs.vagrantup.com/v2/synced-folders/virtualbox.html

## <a name="provisioning">៨. Provisioning</a>

មកដល់នេះ អ្នកបានដំឡើងរួចរាល់VMមួយក្នុងដៃ និងប្រើប្រាស់យ៉ាងងាយស្រួល។ ដោយពឹងទៅលើ sync foler អ្នកdevតាមចិត្តនៅលើhost computer និងស្ងប់ចិត្តជាមួយVMដែលនឹង
ធ្វើបច្ចប្បន្នភាពcodeថ្មីrealtime។ ក៏ប៉ុន្តែVagrantមិនមែនមានតែត្រឹមនេះនោះទេ មួយក្នុងចំនោមចំនុចវិជ្ជមានរបស់vagrantនោះគឺ `provisioning` ។ និយាយបែបងាយយល់គឺ
នៅពេលអ្នកបើកម៉ាស៊ីនឡើងលើកដំបូង vagrantនឹងចាប់ផ្ដើមដំឡើងបណ្ដាsoftwareអោយម៉ាស៊ីនរបស់អ្នក តាមអ្វីដែលបង្កើត។ វិធីងាយស្រួល និងបុរាណបំផុតគឺប្រើប្រាស់ shell script។
ឧទាហរណ៍ ប្រសិនបើអ្នកចង់ដំឡើង git ក្រោយពីបើម៉ាស៊ីនលើកដំបូង អ្នកអាចedit Vagrantfile ដូចខាងក្រោម
```
config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get install git
SHELL
```

លើកដំបូង up ក្រោយពី add provisioning អ្នកនឹងឃើញថាvagrant ដំណើរការដោយស្វ័យប្រវត្តិ ទៅលើshell script ខាងលើ ហើយដំឡើង git អោយអ្នក។ ក្នុងករណី
អ្នកedit provision អ្នកត្រូវតែ​ `up​ vagrant` ជាមួយនឹងparameter - provision។

ស្រដៀងគ្នាដែរ ជាមួយនឹងការដំឡើងពី inline shell គឺអ្នកអាចបង្កើត 1 file scriptដាច់ដោយឡែក ដើម្បីvagrantdដំណើរការពេលrestart។ តែរបៀបប្រើប្រាស់shellជា 
1​របៀបមួយដែលចំនាស់ទៅហើយ ព្រោះសព្វថ្ងៃនេះមានtoolជាច្រើនជួយក្នុងការsetupនៅលើsystemធំៗ។ ២ជំរើសដ៏ពេញនិយមបំផុត ចំពោះvagrantគឺ Chef & Puppet។ 

## <a name="networking">៩. Netwroking</a>

នៅពេលដែលអ្នកបានដំឡើងពេញលេញនូវមជ្ឈដ្ឋាន(environment)អភិវឌ្ឍន៍ web កិច្ចការចុងក្រោយដែលអ្នកត្រូវធ្វើនោះគឺ config network ដើម្បីអាចaccessចូលទៅក្នុងVM
និងដំណើរការ(run)websiteរបស់អ្នក។ របៀបធ្វើងាយស្រួទេ ជាមួយVagrant ដោយអ្នកconfig Vagrantfileដូចខាងក្រោម៖
```
config.vm.network :forwarded_port, host: 4567, guest: 80
```

ចាប់ពីពេលនេះតទៅ accessដល់port 4567របស់server នឹងforwardដល់port 80នៃVM។ ក្រៅពីនេះ​config basic ជាមួយport​​ forward អ្នកអាចconfig លំបាកបន្តិច
តាមឯកសារconfig [Nework](https://www.vagrantup.com/docs/networking/index.html) របស់Vagrant។
 
## <a name="recap">១០. Recap</a>

អត្ថបទនេះបានផ្ដល់អោយមិត្តអ្នកអាន ពីVagrantកំរិតមូលដ្ឋាននៅឡើយ។ ហើយអ្នកក៏អាច ដឹងថាវាជាអ្វី? ដំឡើងម៉ាស៊ីននិមិត្ត១ដូចម្ដេច? ក្នុងអត្ថបន្ត យើងនឹងបន្តស្វែងយល់បណ្ដាfeature
ជ្រាលជ្រៅថែមទៀតអំពិរVagrant​(Provisioning, Multi-Machine, & Boxes)។


## <a name="ref">១១. References</a>

[1] https://viblo.asia/p/tim-hieu-vagrant-phan-1-1l0rvmDQGyqA

[2] https://viblo.asia/p/tim-hieu-vagrant-phan-2-vagrant-chef-wznVGLqQvZOe

[3] https://thachpham.com/tools/tao-may-ao-tren-may-tinh-voi-vagrant-va-virtualbox.html
