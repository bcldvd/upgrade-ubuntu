# Upgrade Ubuntu

## Sommaire

* [Pré-requis](#pre-requis)
* [Firefox](#firefox)
* [SSH](#ssh)
* [GIT](#git)
* [ZSH](#zsh)
* [Vim](#vim)
* [LAMP](#lamp)
* [Chromium](#chromium)
* [Java](#java)
* [Adobe Flash](#adobe-flash)
* [Dropbox](#dropbox)
* [Ruby](#ruby)
* [Node](#node)
* [PostgreSQL](#postgresql)
* [MongoDB](#mongodb)
* [Sublime Text 3](#sublime-text-3)
* [Désactiver les lancements au démarrage](#desactiver-les-lancements-au-demarrage)
* [Créer des lanceurs personnalisés sur Unity](#creer-des-lanceurs-personnalises-sur-unity)
* [Installer d'autres logiciels](#installer-d-autres-logiciels)
* [Autres](#autres)


## Pré-requis

Créer un dossier `workspace` dans le home.

```
mkdir ~/workspace
sudo ln -s ~/workspace/ /workspace
```


## Firefox

- changer l'adresse Home
- configurer Sync

### Configuration des touches

Aller dans *about:config* et mettre les paramètres suivants :

- `browser.backspace_action` à `0`
- `browser.ctrlTab.previews` à `true`

### Installation des plugins

- Firebug
- Live HTTP Headers
- Web Developer
- Adblock Plus
- Pocket
- JSONView
- feedly
- Firefox OS Simulator
- ColorZilla

**Tip** : pour sélectionner la Logitech pour les fichiers apt depuis Firefox, sélectionner le script suivant : `/usr/bin/software-center`.

### Utiliser Aurora

Pour utiliser Firefox Aurora au lieu de la version stable officielle :

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/firefox-aurora
    sudo apt-get update
    sudo apt-get install firefox

### Utiliser Nightly

Pour installer Firefox Nightly (n'écrase pas la version Stable/Aurora) :

    sudo add-apt-repository ppa:ubuntu-mozilla-daily/ppa
    sudo apt-get update
    sudo apt-get install firefox-trunk


## SSH

```
cd ~
ssh-keygen -t rsa
```

Il peut être intéressant d'installer openssh-server : `sudo apt-get install openssh-server`

/!\ Se connecter à Github et Bitbucket et ajouter la clé publique SSH dans l'administration de compte.


## GIT

### Installation

```
sudo apt-get install git
```

### Configuration

```
gedit ~/.gitconfig
```

Ajouter le texte suivant : voir [https://github.com/rhannequin/dotfiles/blob/master/gitconfig](https://github.com/rhannequin/dotfiles/blob/master/gitconfig).

### Vérification

```
cd ~/workspace && mkdir github bitbucket
cd github
git clone git@github.com:rhannequin/upgrade-ubuntu.git
```


## ZSH

```
sudo apt-get install zsh curl
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
chsh -s /bin/zsh
```

Modifier le fichier `~/.zshrc` et remplacer son contenu par : [https://github.com/rhannequin/dotfiles/blob/master/zshrc](https://github.com/rhannequin/dotfiles/blob/master/zshrc).


## Vim

    sudo apt-get install vim
    mkdir ~/.vim && mkdir ~/.vim/bundle
    git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
    vim .vimrc

Voir [https://github.com/rhannequin/dotfiles/blob/master/vimrc](https://github.com/rhannequin/dotfiles/blob/master/vimrc).

Lancer `:BundleInstall`.


## LAMP

### Installation

```
sudo apt-get install lamp-server^
sudo ln -s ~/workspace/ /var/www/workspace
sudo apt-get install phpmyadmin
/* sélectionner "apache2" pour reconfigurer automatiquement */
/* sélectionner "oui" pour configurer phpmyadmin avec dbconfig-common */
sudo ln -s /usr/share/phpmyadmin/ /var/www/phpmyadmin
sudo service apache2 restart
```

#### Configuration de PHP

Il est nécessaire de modifier les fichiers php.ini de PHP pour activer le debug. Les paramètres suivants doivent être tels que :

```
- error_reporting = E_ALL
- display_errors = On
- display_startup_errors = On
- track_errors = On
```

#### Configuration des modules

Il est nécessaire d'activer certains modules avec les commandes ci-dessous :

```
sudo apt-get install curl libcurl3 libcurl3-dev php5-curl php5-mcrypt
sudo a2enmod rewrite deflate
sudo service apache2 restart
```


## Chromium

    sudo apt-get install chromium-browser
    # TODO: fix missing backspace key and ctrl+tab


## Java

[Télécharger](http://www.oracle.com/technetwork/java/javase/downloads/index.html) JAVA SE 7u55 JDK.

```
sudo mkdir /usr/lib/jvm
cd /usr/lib/jvm
sudo tar xvzf ~/jre-7u45-linux-*.tar.gz
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.7.0_55/bin/java 1
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.7.0_55/bin/javac 1
sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.7.0_55/bin/javaws 1
# Pour 64 bits
# sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_55/jre/lib/amd64/libjavaplugin_jni.so 1
sudo update-alternatives --install /usr/lib/mozilla/plugins/libjavaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.7.0_55/jre/lib/i586/libjavaplugin_jni.so 1
sudo update-alternatives --config java
sudo update-alternatives --config javac
sudo update-alternatives --config javaws
sudo update-alternatives --config mozilla-javaplugin.so
```

## Adobe Flash


## Dropbox

32 bits

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

64 bits

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

Puis lancer

    ~/.dropbox-dist/dropboxd


## Ruby

### Pré-requis

Pour installer Ruby, il faut au préalable installer :

```
sudo apt-get install libssl-dev g++
```

### Avec rbenv

```
git clone git://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
source ~/.bashrc
git clone git@github.com:sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 2.1.1
rbenv global 2.1.1
```

### Avec RVM

    sudo apt-get install aptitude samba curl
    \curl -L https://get.rvm.io | bash -s stable --ruby
    rvm requirements
    rvm install 2.1.1

Assurez-vous d'avoir la ligne suivante dans votre `~/.bashrc`:

    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" # This loads RVM into a shell session.

### Installation de Rails

```
sudo apt-get install libsqlite3-dev libpq-dev
gem install rails
# Only for rbenv
rbenv rehash
rails -v
```


## Node

### Installation

```
sudo apt-get install g++
cd ~/workspace
git clone https://github.com/joyent/node.git && cd node
git checkout v0.10.26
mkdir ~/opt && mkdir ~/opt/node
./configure --prefix=~/opt/node
make
make install
```

### Ajout de modules

    npm install -g express coffee-script bower grunt-cli jslint nodemon sails


## PostgreSQL

### Installation

    sudo apt-get install postgresql

### Création d'un nouvel utilisateur

```
sudo -i -u postgres
psql
CREATE USER rhannequin;
ALTER ROLE rhannequin WITH CREATEDB;
CREATE DATABASE first_pq_db OWNER rhannequin;
ALTER USER rhannequin WITH ENCRYPTED PASSWORD 'root';
\q
exit
```

### Pgadmin

`sudo apt-get install pgadmin3`


## MongoDB

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
sudo apt-get update
sudo apt-get install mongodb-10gen
```


## Sublime Text 3

### Installation

    sudo add-apt-repository ppa:webupd8team/sublime-text-3
    sudo apt-get update
    sudo apt-get install sublime-text-installer

### Package Control

Rentrer la commande :

    import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

Si un problème se produit, ce qui semble être fréquent depuis quelques temps, il faut [télécharger le binaire du package](http://sublime.wbond.net/Package%20Control.sublime-package)  puis le déplacer dans le dossier "Installed Packages" du répertoire de configuration de Sublime Text (voir `$HOME/.config`). Redémarrer Sublime.

Si la connexion Internet subit un proxy, il faut configurer modifier le fichier *Preferences > Package Settings > Package Control > Settings - User* par :

```javascript
{
  // An HTTP proxy server to use for requests
  "http_proxy": "xxx.xxx:xx",

  // An HTTPS proxy server to use for requests - this will inherit from
  // http_proxy if it is set to "" or null and http_proxy has a value. You
  // can set this to false to prevent inheriting from http_proxy.
  "https_proxy": "xxx.xxx:xx",

  // Username and password for both http_proxy and https_proxy
  "proxy_username": "xxx",
  "proxy_password": "xxx"
}
```

### Installer les packages

- BracketHighlither
- Better CoffeeScript
- INI
- SCSS
- Markdown Preview
- Trailing Spaces
- Emmet
- Sublime Linter
- SideBarEnhancements
- AdvancedNewFile
- Theme - Flatland

### Paramètres

Remplacer le contenu du fichier *Preferences > Settings - User* par : voir [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/settings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/settings).

Remplacer le contenu du fichier *Preferences > Key Binding - User* par : voir [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/key-bindings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/key-bindings).

#### Sublime Linter

Éditer les *Settings - User de Sublime Linter* : voir [https://github.com/rhannequin/dotfiles/blob/master/sublimetext/sublime-linter-settings](https://github.com/rhannequin/dotfiles/blob/master/sublimetext/sublime-linter-settings).


## Désactiver les lancements au démarrage

    sudo apt-get install sysv-rc-conf
    sudo sysv-rc-conf


## Créer des lanceurs personnalisés sur Unity

```
sudo apt-get install gnome-panel
sudo gnome-desktop-item-edit /usr/share/applications/ --create-new
```


## Installer d'autres logiciels

```
sudo apt-get install indicator-multiload vlc flashplugin-installer meld rar gimp filezilla openvpn virtualbox alacarte
```

### JetBrains

Installer [IntteliJ IDEA](http://www.jetbrains.com/idea/download), [RubyMine](http://www.jetbrains.com/ruby/download) et [WebStorm](http://www.jetbrains.com/webstorm/download) depuis le site de JetBrains.

### Skype

Via le site de skype.

### Heroku

    wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh

### Friends

    sudo apt-get install friends-app dconf-tools
    dconf-editor
    # > com > canonical > friends
    # Mettre interval à 1 et notifications à all


## Autres

- Configurer Twitter
- Paramètres Système > Vie privée : retirer "Enregistrer l'activité" et "Inclure les résultats de recherche".

### Nautilus pour 13.04 et 13.10 (résolu sous 14.04)

Activer le Backspace pour revenir en arrière :

    echo '(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")' >> ~/.config/nautilus/accels

### Déplacer les fenêtres pour 13.10 (résolu sous 14.04)

    sudo apt-get install compizconfig-settings-manager
    ccsm

Aller dans Desktop Wall > Assignations > Move with window within the wall
