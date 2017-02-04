
Installing ASP.NET 5 on Ubuntu 16.04 (Xenial Xerus)
---------------------------------------------------

<h4>Install the .NET Version Manager (DNVM)</h4>

    curl -sSL https://raw.githubusercontent.com/aspnet/Home/dev/dnvminstall.sh | DNX_BRANCH=dev sh && source ~/.dnx/dnvm/dnvm.sh

<h4>Install the .NET Execution Environment (DNX)</h4>

I have installed DNX for the .NET core. For instructions to install DNX for Mono see [install DNX for Mono](http://docs.asp.net/en/latest/getting-started/installing-on-linux.html#install-the-net-execution-environment-dnx "Install DNX for Mono").

Install the DNX prerequisites:

    sudo apt-get install libunwind8 gettext libssl-dev libcurl4-openssl-dev zlib1g libicu-dev uuid-dev

Use DNVM to install DNX for .NET Core:

    dnvm upgrade -r coreclr

<h4>Install libuv</h4>

Kestrel is a cross-platform HTTP server and the project is part of ASP.NET Core. Kestrel is based on Libuv,  a multi-platform asynchronous IO library.

    sudo apt-get install make automake libtool curl
    curl -sSL https://github.com/libuv/libuv/archive/v1.8.0.tar.gz | sudo tar zxfv - -C /usr/local/src
    cd /usr/local/src/libuv-1.8.0
    sudo sh autogen.sh
    sudo ./configure
    sudo make
    sudo make install
    sudo rm -rf /usr/local/src/libuv-1.8.0 && cd ~/
    sudo ldconfig


If the DNX and DNU commands produce no output, install libicu52 manually [Source](https://github.com/aspnet/dnx/issues/3059#issuecomment-150672608):

    wget http://security.ubuntu.com/ubuntu/pool/main/i/icu/libicu52_52.1-8ubuntu0.2_amd64.deb

    dpkg -i libicu52_52.1-8ubuntu0.2_amd64.deb

----------

<h3>Scaffolding Applications Using Yeoman</h3>

<h4>Install Node.js, npm, and Yeoman</h4>
If you don't have Node.js installed, follow the instructions on [nodejs.org](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions). The installer includes both Node.js and npm.

Install yo, bower, grunt, and gulp:

    npm install -g yo bower grunt-cli gulp-cli

Install the ASP.NET generator:

    npm install -g generator-aspnet

----------

<h3>Create an ASP.NET application</h3>

Create a new directory for your project and navigate to the project.

To create a new ASP.NET project:

    yo aspnet

Navigate down and select the Web Application project. The ASP.NET generator creates ASP.NET 5 DNX project that can be loaded into Visual Studio 2015 or run from the command line.


![enter image description here](https://www.dropbox.com/s/tz5dyx514ns5ohr/yoman-aspnet-generator.gif?dl=0)
Image src: [Create an ASP.NET app](http://docs.asp.net/en/latest/client-side/yeoman.html#create-an-asp-net-app)

Restore the project’s NuGet package dependencies:

    dnu restore

Build the project:

    dnvm use default -r coreclr && dnu build --framework dnxcore50

Running locally using Kestrel:

    dnx web

From your browser, navigate to `http://localhost:5000`
