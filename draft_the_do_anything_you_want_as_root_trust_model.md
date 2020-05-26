Title: The "do anything you want as root" trust model

`apt-get` is heavily used on linux systems, and honestly, it's a pretty neat tool. By default, it will only install from official ubuntu repositories, and enforce signature validation, making mitm 

1. you trust a repository by adding it to /etc/apt/sources.list. Trusting the repository means you trust if you ask for a specific package name, it will send you a true copy of that package. It doesn't mean that you like every package in that repo.
2. you may need a credential to access a private repository. you specify how to authenticate to a repository by adding adding a line like "deb ssh …" or "deb ftp..." to /etc/apt/sources.list. You also distribute a password to the client or client's public key to the server.
3. You implicitly trust a package and future versions of it by apt-get installing it. This is the point where you decide if the individual package is secure enough for your taste, if it was written by people you trust, etc.
4. You typically run apt-get update periodically without reevaluating your trust in the repository or your selected pacakges.


