# Arch Linux Catapult

## What is Arch Linux Catapult
Arch Linux Catapult (ALC) is a proof of concept for a next generation installer for Arch Linux.
Usually a user would download the official Arch Linux ISO file, boot it, and type in all commands necessary
for their installation.

Arch Linux Catapult follows a new approach. It "catapults" an Arch Linux image on the hard drive.
This process will need a working internet connection and a bootable Arch Linux ISO.
The only difference is that you don't need to install Arch Linux in the complicate way, you can just install
an Arch Linux image with your favourite flavor and push it directly on the hard disk.

It's basically this:
```
root@iso # curl <URL to the ALC images>/kde.img -O
root@iso # curl <URL to the ALC images>/kde.img.sig
root@iso # gpg --verify kde.img.sig
root@iso # dd if=kde.img of=/dev/sda
```

When the copy process has finished, you are able to boot into your personal Arch Linux installation.
You will get greeted by `systemd-firstboot` and you will get asked for various options, you may want to set:

* root users password
* system hostname
* system timezone
* system keyboard map
* system locales

During boot `systemd-resizefs` will automatically resize your root partition and enlarge the existing installation.
Thus we can use a 2GB image and bootstrap a full system with 1TB of size or even more (depending on the chosen filesystem).

Right now my dream looks as follows:

* I provide basic images with common Desktop environments and a X/wayland-less server image
* In the meanwhile this repository is open for PRs for `community` images. You can build your own mkosi files,
I will check them for security violations, possible sign them (this is not yet decided), build them and upload them to
a host for serving them via HTTPS.
* I would also like to have a small client (maybe ncurses) for listing all available images, choosing your right image, validating
the signature and installing it on the hard drive. Everything with one `enter` push.

## Is this project an official Arch Linux project

No, and I don't know if it ever will be.

## How is it working?

I generate images with [mkosi](https://github.com/systemd/mkosi). mkosi is able to generate raw gpt images,
these images can be even encrypted with LUKS.

## Are there any restrictions?

Yes, mkosi can build only UEFI images, also I didn't figure out a way yet for supporting dualboot.
Therefore this approach or installation method applies to your requirements only if you want to use your full disk space.
Also, there is no support for Raids.

## Why are you doing this?

I want to make Arch Linux less elitist with this approach, more people should be able to use it, if they want to do it.
Nobody should be forced to read 2 hours of the Arch Wiki before installing Linux on their machine.

(I don't say I don't like the ArchWiki, the Wiki is awesome and I think everybody should at least try to understand what they are actually doing).

## Sounds cool, how can I contribute?

Just open a PR against my `community` directory in this repository. If this projects finds enough contributors, I will open a Github organization
for this project. I am also looking for a nice logo, if you are an artist and you want to stick your art on this project. Feel free to open a PR
for this README.

## What do I need for building images?

You just need mkosi from the AUR (I hope I can bring mkosi to the [community] repository soon).
If you need an example file, have a look in repository, there should be plenty of them.

Besides this there is more going on behind the curtain. I've made a firstboot patch for mkosi and systemd is working on the resizing
part of the partition table. (so no need for cloud-init).

* https://github.com/systemd/mkosi/pull/385
* https://github.com/systemd/systemd/issues/14052

## Disclaimer

This project is just a proof of concept. If you feel offended because I break your elitist way to install Arch, feel free to do so.
It's your right and your opinion, but don't bug me with your opinion.
