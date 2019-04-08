Auto-Config
======

Fair warning, this is very much still experimental and not in regular use. My goal here is to put together a script that can be used to configure a brand new macOS laptop/desktop computer with all the necessary (mostly open source) tools to do my work. This is mostly academic writing (using sublime text, pandoc and marked), data science (in R and Python), a fair bit of command-line work in iTerm and web publishing.

This is a mashup of various scripts out in the wild. I suggest that an curious individual consult a few websites to get a proper briefing on how dotfiles/laptop configuration scriptus work:

- <http://dotfiles.github.io/>
- <https://github.com/webpro/awesome-dotfiles>

My main forks for this script come from:

- <https://github.com/thoughtbot/laptop> for the initial work
- Subsequent changes to this script have been integrated from <https://github.com/monfresh/laptop> as well, particularly for the python environment and homebrew installs.
- Sensible defaults for macos automated by <https://github.com/mathiasbynens/dotfiles>
- Templates for pandoc from <https://github.com/kjhealy/pandoc-templates>

At least in principle, it can be run multiple times on the same machine safely. It installs, upgrades, or skips packages based on what is already installed on the machine.

Requirements
------------

We support:

* macOS Mavericks (10.9)
* macOS Yosemite (10.10)
* macOS El Capitan (10.11)
* macOS Sierra (10.12)
* macOS High Sierra (10.13)
* macOS Mojave (10.14)

Older versions may work but aren't regularly tested.
Bug reports for older versions are welcome.

Install
-------

Download the script:

```sh
curl --remote-name https://raw.githubusercontent.com/kidwellj/laptop/master/mac
```

Review the script (avoid running scripts you haven't read!):

```sh
less mac
```

Execute the downloaded script:

```sh
sh mac 2>&1 | tee ~/laptop.log
```

Optionally, review the log:

```sh
less ~/laptop.log
```


Debugging
---------

Your last Laptop run will be saved to `~/laptop.log`.
Read through it to see if you can debug the issue yourself.
If not, copy the lines where the script failed into a
[new GitHub Issue](https://github.com/thoughtbot/laptop/issues/new) for us.
Or, attach the whole log file as an attachment.

What it sets up
---------------

macOS tools:

* [Homebrew] for managing operating system libraries.

[Homebrew]: http://brew.sh/

Unix tools:

* [Git] for version control
* [OpenSSL] for Transport Layer Security (TLS)
* [RCM] for managing company and personal dotfiles
* [Tmux] for saving project state and switching between projects
* [Zsh] as your shell

[Git]: https://git-scm.com/
[OpenSSL]: https://www.openssl.org/
[RCM]: https://github.com/thoughtbot/rcm
[Tmux]: http://tmux.github.io/
[Zsh]: http://www.zsh.org/

Text tools and writing:

* [Pandoc] for [MarkDown] rendering to docx and pdf
* [CsvFIX]

[Pandoc]: https://pandoc.org/
[Markdown]: https://daringfireball.net/projects/markdown/

Geospatial tools:

* [gdal]

Media tools:

* [ImageMagick] for cropping and resizing images
* [FFMPEG] for modifying audio files
* [tesseract]

Programming languages, package managers, and configuration:

* [ASDF] for managing programming language versions
* [Bundler] for managing Ruby libraries
* [Node.js] and [NPM], for running apps and installing JavaScript packages
* [Ruby] stable for writing general-purpose code
* [Yarn] for managing JavaScript packages
* [R] and [Python] for data science

[Bundler]: http://bundler.io/
[ImageMagick]: http://www.imagemagick.org/
[Node.js]: http://nodejs.org/
[NPM]: https://www.npmjs.org/
[ASDF]: https://github.com/asdf-vm/asdf
[Ruby]: https://www.ruby-lang.org/en/
[Yarn]: https://yarnpkg.com/en/

Databases:

* [Postgres] for storing relational data
* [PostGIS] extensions for geospatial data
* [Redis] for storing key-value data

[Postgres]: http://www.postgresql.org/
[PostGIS]: https://postgis.net/
[Redis]: http://redis.io/

It should take less than 15 minutes to install (depends on your machine).

Customize in `~/.laptop.local`
------------------------------

Your `~/.laptop.local` is run at the end of the Laptop script.
Put your customizations there.
For example:

```sh
#!/bin/sh

brew bundle --file=- <<EOF
brew "Caskroom/cask/dockertoolbox"
brew "go"
brew "ngrok"
brew "watch"
EOF

default_docker_machine() {
  docker-machine ls | grep -Fq "default"
}

if ! default_docker_machine; then
  docker-machine create --driver virtualbox default
fi

default_docker_machine_running() {
  default_docker_machine | grep -Fq "Running"
}

if ! default_docker_machine_running; then
  docker-machine start default
fi

fancy_echo "Cleaning up old Homebrew formulae ..."
brew cleanup
brew cask cleanup

if [ -r "$HOME/.rcrc" ]; then
  fancy_echo "Updating dotfiles ..."
  rcup
fi
```

Write your customizations such that they can be run safely more than once.
See the `mac` script for examples.

Laptop functions such as `fancy_echo` and
`gem_install_or_update`
can be used in your `~/.laptop.local`.

See the [wiki](https://github.com/thoughtbot/laptop/wiki)
for more customization examples.


License
-------

Part of this script are covered by a creative commons license by thoughtbot, inc. Those bits are nonetheless free software and may be redistributed under the terms specified in the [LICENSE] file.

[LICENSE]: LICENSE