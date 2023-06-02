Gnome
=========

Gnome Shell configurations and tweeks.
This role will configure and install gnome theme and icons, it also allows to customize settings
using `dconf`.  

By default this role will install the [dracula gtk theme](https://draculatheme.com/gtk), but any custom theme can be installed
by changing the role variables.  

Requirements
------------

* ansible >= 2.11.12
* community.general >= 3.8.3

Role Variables
--------------

All the supported variables can be listed from `defaults/main.yml` file, here is more info about it:  


**The theme section**  
* `gnome_setup_packages`  - `(list)` - list of packages to be installed  
* `gnome_install_theme`   - `(bool)` - enable or disable the theme installation (defaults `true`)  
* `gnome_install_theme_cleanup` - `(bool)`  - toggle the cleanup of temporary theme files after installation  
* `gnome_enable_theme`  - `(bool)`  - toogle the theme enablement. `(default true)` will auto enable the theme after installation._A session reload may be required, such as pkill -1 gnome-shell_  
* `gnome_theme_name` - `(str)` - the theme name as simple string. `(default 'Dracula')`.  
* `gnome_theme_url` - `(str)` - the full URL to download the theme file, it must be a `zip` or a tarball file. `(default to dracula URL)`.  
* `gnome_theme_install_dir` - `(str)` - the full path where theme will be installed. `(default $HOME/.themes)` to install in system use `/usr/share/themes/`  

**The icon section**  

* `gnome_install_icon_theme`  - `(bool)`  - toggle the icon theme installation. `(default true)`.  
* `gnome_enable_icon_theme`   - `(bool)`  - enable or disable the icon theme after installation. `(default true)`.  
* `gnome_icon_theme_name`   - `(str)` - the icon theme name `(default Dracula)`.  
* `gnome_icon_theme_url`  -   `(str)` - the icon theme URL to be downloaded. Files must be a `zip` or a tarball. `(defaults to dracula icons)`.  
* `gnome_icon_theme_install_dir`  `(str)` - the path where the icon theme will be installed. `(default $HOME/.icons)`, to install as system global use `/usr/share/icons/`


Dependencies
------------

None


Example Playbook
----------------

* Using the role with all the default settings to install the dracula theme:  
```yaml
    - hosts: servers
      roles:
         - { role: mrbrandao.gnome }
```

* Installing a custom theme from gnome-look:  

```yaml
# working in progress
---
- name: "Installing mrbrandao.gnome role"
  hosts:
    - servers
  vars:
    gnome_theme_name: "MacOSX-Mojave"
    gnome_theme_url: ""
    gnome_icon_theme_name: "MacOSX-Mojave"
    gnome_icon_theme_url: ""
  roles:
    - mrbrandao.gnome
```

* Installing the theme as a system global theme:  

```yaml
---
- name: "Installing mrbrandao.gnome role"
  hosts:
    - servers
  vars:
    gnome_theme_name: "MacOSX-Mojave"
    gnome_theme_url: "https://full-url-of-my-theme.zip"
    gnome_theme_install_dir: "/usr/share/themes/"
    gnome_icon_theme_name: "MacOSX-Mojave"
    gnome_icon_theme_url: "https://full-url-of-icon-theme.zip"
    gnome_icon_theme_install_dir: "/usr/share/icons/"
  roles:
    - mrbrandao.gnome
```


* Installing the theme using ansible-vault in the `become-pass`:  

This role will need the `become` during the package installation phase, if wondering to auto inform
the `become-pass` from the YAML use `ansible-vault` and configure the yaml as bellow:  

1. `echo "myvaultpass" > vault` - saving your vault pass  
2. `echo 'pass: MyBecomePass' > secret.yml` - creating the vault secret  
3. `ansible-vault encrypt secret.yml` - encrypt it using your vault pass  
4. `ansible-playbook gnome-play.yml --vault-password-file=vault` - run your gnome playbook using your vault pass.  


```yaml
---
# gnome-play.yml example with become pass as a vault secret
- name: "Installing mrbrandao.gnome role"
  hosts:
    - servers
  vars:
    ansible_become_pass: "{{ pass }}"
  vars_files:
    - secret.yml

  roles:
    - mrbrandao.gnome
```

License
-------

GPL-3.0-only

Author Information
------------------

@mrbrandao - Igor Brandao - https://github.com/mrbrandao
