Sigil66.com APT Publishing Job 
===============================

Present here in case it might be useful for someone else.

#### Instructions

*Setup Build System*
```
sudo apt-get install ruby2.3 ruby2.3-dev build-essential s3cmd
sudo gem install bundler
```
*Build/Publish APT Repo*
```
cd <path to project>
rake
```
