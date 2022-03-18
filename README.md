# ruby-install-M1-Mac

## Requirements:
* Homebrew
* RVM
* Ruby

## Installation:

### Prerequisites

Before start installation ruby, you must:

1. Right click Terminal from the Application/Utilities folder, Get Info, tick the "Open using Rosetta" box.
2. Uninstall Homebrew 
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall.sh)"
    rm -rf /opt/homebrew/*
    sudo rm -rf /opt/homebrew
    ```
## Install Homebrew

1. Installation command
    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
2. Restart terminal
3. Check Homebrew is working fine: `brew doctor`

### Install RVM:

1. Install **RVM**
    ```bash
    \curl -sSL https://get.rvm.io | bash -s stable
    ```
2. Install **OpenSSL**:
    ```bash
    brew install openssl@1.1
    export PATH="$(brew --prefix)/opt/openssl@1.1/bin:$PATH"
    export LDFLAGS="-L$(brew --prefix)/opt/openssl@1.1/lib"
    export CPPFLAGS="-I$(brew --prefix)/opt/openssl@1.1/include"
    export PKG_CONFIG_PATH="$(brew --prefix)/opt/openssl@1.1/lib/pkgconfig"
     ```
3. Disable rvm autolibs - `rvm autolibs disable`
### Ruby Installation
1. Export ruby complier flags
    ```bash
    export RUBY_CFLAGS=-DUSE_FFI_CLOSURE_ALLOC
    export optflags="-Wno-error=implicit-function-declaration"
    ```
2. ruby installation command
    ```bash
    rvm install 2.7.3 --with-openssl-dir=$(brew --prefix)/opt/openssl@1.1
    ```  
3. Post installation check - `rvm list`

    ![Alt text](https://github.com/Na-Sathish/ruby-install-M1-Mac/blob/master/ruby-version-image.png "Ruby Version & Processor Check")
4. Go to application directory and run `bundle install`

## Issues occur while bundle install

1. Getting libv8 gem error. Install libv8 gem via `gem install libv8 -v '3.16.14.19' -- --with-system-v8`
2. Getting mysql gem error.
    * Install mysql `bundle install mysql` (Default it will install mysql 8.* version).
    * For mysql client alone `brew install mysql-client`
    - [Install MYSQL-5.7 Steps on Mac M1](#Install-MYSQL-5.7-Steps-on-Mac-M1)
3. Getting therubyracer gem error.
    ```bash
    brew install v8@3.15
    bundle config build.libv8 --with-system-v8
    bundle config build.therubyracer --with-v8-dir=$(brew --prefix v8@3.15)
    ```
    
## Install MYSQL-5.7 Steps on Mac M1

1. Download mysql package from official page - [Click Here](https://downloads.mysql.com/archives/community/)
2. Select product version as 5.7.31 & Operating system as MacOS
3. Download `macos10.14-x86_64.dmg` or [Click Here](https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.31-macos10.14-x86_64.dmg)
4. Follow the install instructions as same as package installer.
5. **While installing mysql temporary password will be generated. Please note down the password**
6. After successfull installation, connect the mysql client using temporary password.
7. Setting up local mysql client authentication
    * Without or Empty Password:
        ```sql
        SET password="";
        UPDATE mysql.user SET authentication_string=PASSWORD("") WHERE User='root';
        ```
    * With Passoword:
        ```sql
        SET password="<your_passowrd>";
        UPDATE mysql.user SET authentication_string=PASSWORD("<your_passowrd>") WHERE User='root';
        ```
