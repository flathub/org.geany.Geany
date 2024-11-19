# Information for future maintaners

This is a document explaining why certain things in this repo are the way they are, for the benefit of whoever next inherits the maintenance of this package. This was first written by the maintainer from November 2021 to November 2024, [TiZ](https://github.com/TiZ-HugLife). Future maintainers and Flathub staff should feel free to update this file as they see fit.

## Why FDO runtime?

It's partially ideological, partially practical. Geany is a desktop-agnostic GTK3 application, and TiZ believed that desktop-agnostic applications fit best with the FDO runtime. It is a convention already followed with similar applications like Firefox.

But there are practical benefits, too. Most of all, the desktop-specific runtimes go end-of-life twice as fast as FDO does. The current version of geany-plugins can't build on FDO 24.08 due to compiler errors on GCC 14. But FDO 23.08 won't go end-of-life until FDO 25.08 comes out. By then, hopefully there will be a release that addresses errors on newer compilers.

## What's up with webkitgtk?

Geany-plugins's dependency on webkitgtk would be a great reason to use the GNOME runtime despite Geany being desktop-agnostic, but GNOME doesn't ship the webkitgtk that Geany needs.

All of the plugins that use libsoup use the older 2.x version, and GNOME's GTK3 webkit uses libsoup 3.x. You can't mix libsoup versions in one application, so we have to build webkit from scratch to use libsoup2. If Geany-plugins completes migration to libsoup3--something that is [slowly in progress](https://github.com/geany/geany-plugins/pull/1342)--using the GNOME runtime should definitely be revisited, because building webkitgtk is hands-down the *very worst* part of building this package locally.

## What's the patch for?

`native_dialogs.patch` adds file chooser portal support to Geany before it's officially available in the 2.1 release, because folks using Flatpak applications generally expect their platform-native file dialogs to appear. Geany's custom GTK3 file chooser allows changing the encoding of a file as you open it, which is a valid but niche use case.

Geany's developers believe this case is more important than platform-native file dialogs, thus on Linux, this is disabled by default. So there's also a sed substitution in the manifest to enable it by default for this package.

## Is there contention with the Geany developers?

Kinda-sorta-not-really. TiZ was tired of a bunch of other stuff in the Linux app ecosystem, and this was just the straw that broke his back. There is less contention and hard feelings among the actual stakeholders than it seems from the fact that this is TiZ's last contribution to this package.

Geany's developers are kinda-sorta at odds with Flathub because they have made a request that is impossible to accommodate, due to ideological differences between them and the stewards of Flathub. [The discussion is here,](https://github.com/flathub/org.geany.Geany/issues/92) but to summarize it, they want the Flathub page to display the maintainers of the package, but that's not what the developer field of the Flathub app page is supposed to do. It's supposed to show the people *actually developing* the software. However, the Flathub page doesn't meaningfully display the maintainers of a package, because they're sort of committed to a false dichotomy: maintained by the developers vs maintained by the community. This dichotomy and its implications are exacerbated by their terminology for it: verified vs unverified. This terminology stinks because it's not reflective of reality, or the way that distributions and other packaging formats operate. Unfortunately, [they are not interested in changing anything about it.](https://github.com/flathub-infra/website/issues/3131)

Thus, it is impossible to satisfy either stakeholder in this conflict.

This is the least relevant thing to actually maintaining the package, but anyone who does pick this up may have to deal with it, so I (TiZ) thought it at least worth mentioning and summarizing.
