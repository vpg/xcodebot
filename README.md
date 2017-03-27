![](https://img.shields.io/badge/gem-2.6.8-E9573F.svg)
![](https://img.shields.io/badge/xcode-8.2-blue.svg)

**Xcodebot** is a non-official cross-platform tool for managing Xcode server on *any server environment* (only Ruby is required), thus it can be used on Jenkins server even if Xcode is not installed on the machine.

Whereas Xcode API was ASYNCHRONOUS, the provided bot provides a way for calling bots completely SYNCHRONOUSLY.

Some ideas for using it :

- bidirectionnal communication between XcodeServer and CI server
- call Xcodebot on any environment with or without Xcode
- display Xcodebot results in a CLI (rather than an ugly JSON)
- ...

Feel free to open issues / open PRs.

## Installation

> gem install xcodebot

Or :

> gem "xcodebot", :git => "git://github.com/vpg/xcodebot.git"

Some environment variables should be set for having access to your Xcode server :

> export XCODEBOT_EMAIL="user@mail.com"  
> export XCODEBOT_PASSWORD="MY_PASSWORD"  

In this way, your credentials are never stored in any configuration file and you keep any control on them.

## Usage

> xcodebot --help

| Actions      | Description                      |
|--------------|----------------------------------|
| config       | Configure Xcode server           |
| bots         | Manage xcode bots (CRUD)         |
| integrations | Manage xcode integrations (CRUD) |

### Configuration (`config`)

> xcodebot help config

| ARGS                 | Description                               |
|----------------------|-------------------------------------------|
| --list, -l           | Display xcode server api endpoint         |
| --address, -a        | Set address for xcode server              |
| --localhost, --local | Use localhost as address for xcode server |

### Bots (`bots`)

> xcodebot help bots

| ARGS               | Description                 | Parameters    |
|--------------------|-----------------------------|---------------|
| --list, -l         | List all bots               | -             |
| --get              | Get information about a bot | <bot_id>      |
| --stats, -s        | Statistics of bot           | <bot_id>      |
| --create, -c       | Create a new bot            | *See above*   |
| --duplicate, -d    | Duplicate bot               | <bot_id>      |
| --delete, --remove | Remove one or several bot   | <bot_id1> ... |

For creating a bot, you need some extra params because of Xcode bot. *Blueprint id* is one of them. Blueprint
information are saved in a file `.xcscmblueprint` which contains some information about your versionned repository
(git or SVN). Some variable are usefull for setting which repo and branch to checkout for example. But note this file
will be different for all developers machine, so a good solution it's using one generated by your Xcode server.

For finding this file, you can run following command on your source repository :

> find . -name '*.xcscmblueprint'

Then you can create a bot by running command (**simplification work in progress**) :

> xcodebot bots --create \\  
> &nbsp;&nbsp;&nbsp;&nbsp;name:'Auto Bot' \\  
> &nbsp;&nbsp;&nbsp;&nbsp;schedule:2 \\  
> &nbsp;&nbsp;&nbsp;&nbsp;clean:1 \\  
> &nbsp;&nbsp;&nbsp;&nbsp;branch:5.7 \\    
> &nbsp;&nbsp;&nbsp;&nbsp;scheme:CI \\  
> &nbsp;&nbsp;&nbsp;&nbsp;blueprint:82EF22EF5CE4E343B67A9F79130BD862EF58AE20 \\  
> &nbsp;&nbsp;&nbsp;&nbsp;project:'VoyagePrive' \\  
> &nbsp;&nbsp;&nbsp;&nbsp;folder:'iosVoyagePrive/' \\  
> &nbsp;&nbsp;&nbsp;&nbsp;git:'github.com:vpg/iosVoyagePrive.git' \\  
> &nbsp;&nbsp;&nbsp;&nbsp;path:'VoyagePrive/VoyagePrive.xcworkspace'

Or with a json model :

> xcodebot bots --create \\  
> &nbsp;&nbsp;&nbsp;&nbsp;name:'Auto Bot' \\  
> &nbsp;&nbsp;&nbsp;&nbsp;schedule:2 \\  
> &nbsp;&nbsp;&nbsp;&nbsp;clean:1 \\  
> &nbsp;&nbsp;&nbsp;&nbsp;branch:5.7 \\    
> &nbsp;&nbsp;&nbsp;&nbsp;scheme:CI \\
> &nbsp;&nbsp;&nbsp;&nbsp;json:vpg_bot.json

### Integrations (`integrations`)

> xcodebot help integrations

| ARGS                       | Description                        | Parameters |
|----------------------------|------------------------------------|------------|
| --cancel                   | Cancel an integration              | <inte_id>  |
| --create, -c               | Create a new integration for a bot | <bot_id>   |
| --delete, --remove         | Remove integration                 | <inte_id>  |
| --delete-all, --remove-all | Remove all integrations            | -          |
| --list, -l                 | List integrations for a bot        | <bot_id>   |
| --logs                     | Get logs of an integration         | <inte_id>  |
| --status, -s               | Get status of an integration       | <inte_id>  |
| --wait, -w                 | Task done synchronously            | -          |

## References

### API Reference

- An outdated (but official) [Apple documentation](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/XcodeServerAPIReference/Bots.html#//apple_ref/doc/uid/TP40016472-CH2-SW1)
- A more detailled (but unofficial) doc from [Buildasaurs/XcodeServer-API-Docs](https://github.com/buildasaurs/XcodeServer-API-Docs).

### Outdated but usefull repo

These (outdated) repositories have been a great source of inspiration for creating this repo. Great thanks to them.

- https://github.com/buildasaurs/XcodeServer-API-Docs
- https://github.com/buildasaurs/xcskarel
- https://github.com/oarrabi/xserverpy

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/jhanzo/xcodebot. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
