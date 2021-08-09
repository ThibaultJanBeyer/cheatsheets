Just another collection of CheatSheets.  
**living document – constantly updating**

# Cheat Sheets

So far:

- [Docker](/docker-cheatsheet.md)
- [Kubernetes](/kubernetes-cheatsheet.md)
- [Nginx](/nginx-cheatsheet.md)
- [Markdown](/Markdown-Cheatsheet.md)
- [JavaScript](/JavaScript-Cheatsheet.md)
- - [Angular](/Angular-Cheatsheet.md)
- [Ruby](/Ruby-Cheatsheet.md)
- - [Rails](/Ruby-on-Rails-Cheatsheet.md)
- [SQL](/sql.md)
- [Command Line](/Command-Line-Cheatsheet.md)
- - [Ubuntu](/ubuntu-cheatsheet.md)
- - [Git](/Git-Cheatsheet.md)
- [Python](/python-cheatsheet.md)
- - [Django](/django-cheatsheet.md)
- [Firebase](/firebase-cheatsheet.md)
- [Unity Mirror](/unity-mirror-cheatsheet.md)
- [Solidity](/solidity-cheatsheet.md)

# Helpers

## Meta Files

### React

- [root](/meta-files/react/)
- - [vscode settings](/meta-files/react/.vscode)
    Sets every `.js` file to be read as `reactjs`.
- - [.env](/meta-files/react/.env)
    Adds `NODE_PATH=src/js/` to be able to cross-import files.
- - [.eslintrc](/meta-files/react/.eslintrc.json)
- - [.prettierrc](/meta-files/react/.prettierrc)

# Theory

- [Identifying Security Vulnerabilities](/theory/identifying-security-vulnerabilities.md)
- [Entity Relationship Diagram (with cardinalities)](/theory/entity-relationship-diagram.md)

## Common

### OSX Bash script not executed "command not found".

```
sudo chmod a+x ./app/build.sh
```

### Install nodejs on ubuntu

https://github.com/nodesource/distributions/blob/master/README.md#debinstall

```bash
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Kill open port in OSX

```bash
sudo lsof -i :<port>
sudo kill -9 <port PID>
```
