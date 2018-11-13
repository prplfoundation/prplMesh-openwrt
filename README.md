# OpenWrt packages for prplMesh

This feed contains packages for enabling WFA Multi-AP on a router.

This work was sponsored by [prpl Foundation](https://prplfoundation.org/).


## What is WFA Multi-AP?

WFA Multi-AP networks utilize multiple APs that work together to ensure all areas of the home have complete Wi-Fi coverage and adapt to changing network conditions. See the links below for details.

- https://www.wi-fi.org/file/wi-fi-certified-easymesh-technology-overview
- https://www.wi-fi.org/discover-wi-fi/specifications
- https://www.wi-fi.org/news-events/newsroom/wi-fi-certified-easymesh-delivers-intelligent-wi-fi-networks
- https://www.wi-fi.org/discover-wi-fi/wi-fi-easymesh
- https://www.wi-fi.org/file/infographic-wi-fi-certified-easymesh
- https://www.wi-fi.org/file/wi-fi-certified-easymesh-highlights


## How to add the prplMesh Feed to you OpenWrt/LEDE build
At the root of your OpenWrt/LEDE tree, add the following to your `feeds.conf` file:
```sh
src-git prplmesh https://github.com/prplfoundation/prplMesh-openwrt.git
```
Now to add the packages on your prplmesh feed to your OpenWrt/LEDE instance:
```sh
./scripts/feeds update prplmesh #retrieve the prplmesh feed from service/update to latest
./scripts/feeds install -p prplmesh #make all of the prplmesh feed packages available to the build
```

For more control over the package versions being installed, you can fork the feed using Github (and replace the `src-git` url) or maintaining a copy of the feed on your local system by using this line instead:
```sh
src-link prplmesh /full/path/to/feed/root
```

