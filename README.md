# Laravel Homestead Magento 2 Config

*Requires Homestead >=3.0*

[Laravel Homestead](https://laravel.com/docs/homestead) is a well tested development environment which provides modern tools to develop PHP applications. Its combination of Nginx as primary web server, and Laravel-focused default config, make it difficult to get more complex php applications like Magento running.

This script allows you to create a valid NGINX server config for your Magento 2 application, ensuring this config remains across reprovisioning (unlike editing the nginx.conf directly on the VM)

The nginx configuration is based on the nginx.conf.sample file from the [Magento 2 Community Edition main repo](https://github.com/magento/magento2)

If you have never used Homestead before, you might be better suited with the purpose-built [Magento Dev Box](https://github.com/magento/magento2devbox-web)

## Usage

Locate the `Homestead` directory where the vagrantfile and `scripts` folder is located - this will commonly be in `~/Homestead/` for users who install it globally.

Within the `scripts` folder, copy in `serve-magento.sh` from this project

You may now specify `magento` as a valid type in your `Homestead.yaml` file

The configuration of your Homestead.yaml file will dictate the paths you use here (depends on your shared folders and their respective mount points)

See the [main Homestead docs](https://laravel.com/docs/5.5/homestead) for further configuration help.

An example using the default Homestead.yaml is given below

```
# Rest of Homestead.yml
# ...
#
# Assumes default 'code' shared folder
sites:
    - map: magento2.test
      to: /home/vagrant/code/magento2
      type: magento
#
# more sites, config etc
```

Then provision the site as per normal
```
~/.Homestead/homestead provision
```
or
```
~/.Homestead/homestead up --provision
````
If you have [installed the `homestead` global bash alias](https://laravel.com/docs/5.5/homestead#accessing-homestead-globally) then the paths don't matter

## Customisation

If you need custom Nginx rules then you can edit the main block to include them - keep in mind that the provisioner is in bash, and has variables provided by Homestead.yaml.

If you need the Homestead.yaml variables then user regular `$1` vars, if you're trying to reference Nginx variables such as `$uri` then be sure to escape the first character - `\$uri`

## Contributing

If you have some improvments or suggestions I'm all ears, however I will not be offering support for this project as Homestead and VM configuration can vary so much from machine to machine. YMMV, good luck out there.

## Y Tho?

For me, the Mage DevBox and Magestead projects are in too much flux and have been unreliable in getting a dev environment up and running consistently.

Homestead has been rock solid and hits the important notes for Magento 2 dev - PHP 7.1, Composer, MySQL, Nginx, Node, Redis etc.

## Homestead + Magento tips

If this is your first time trying to run Magento on Homestead, I'd reccomend ssh-ing into the box and running the following commands before installing Magento
```
sudo apt-get upgrade
sudo apt-get update
sudo apt- get install php7.1-mcrypt php7.1-curl php7.1-intl php7.1-xsl php7.1-mbstring php7.1-zip php7.1-bcmath php7.1-iconv php7.1-soap
```

I'd also recommend the following flags when using the Magento metapackage installer

```
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition <installation directory name> --ignore-platform-reqs --prefer-dist
```

Finally, remember to edit your config to place the site into development mode - either via a config file or using the magento CLI

