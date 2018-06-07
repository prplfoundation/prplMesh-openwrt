# OpenWrt/LEDE packages for Wi-Fi EasyMesh
This feed contains packages for enabling Wi-Fi EasyMesh on a router.

This work was sponsored by [prpl Foundation](https://prplfoundation.org/).


## What is EasyMesh?

Wi-Fi EasyMesh networks utilize multiple APs that work together to ensure all areas of the home have complete Wi-Fi coverage and adapt to changing network conditions. See the links below for details.

- https://www.wi-fi.org/file/wi-fi-certified-easymesh-technology-overview
- https://www.wi-fi.org/discover-wi-fi/specifications
- https://www.wi-fi.org/news-events/newsroom/wi-fi-certified-easymesh-delivers-intelligent-wi-fi-networks
- https://www.wi-fi.org/discover-wi-fi/wi-fi-easymesh
- https://www.wi-fi.org/file/infographic-wi-fi-certified-easymesh
- https://www.wi-fi.org/file/wi-fi-certified-easymesh-highlights


## How to add the EasyMesh Feed to you OpenWrt/LEDE build
At the root of your OpenWrt/LEDE tree, add the following to your `feeds.conf` file:
```sh
src-git easymesh https://github.com/prplfoundation/easymesh-openwrt.git
```
Now to add the packages on your easymesh feed to your OpenWrt/LEDE instance:
```sh
./scripts/feeds update easymesh #retrieve the easymesh feed from service/update to latest
./scripts/feeds install -p easymesh #make all of the easymesh feed packages available to the build
```

For more control over the package versions being installed, you can fork the feed using Github (and replace the `src-git` url) or maintaining a copy of the feed on your local system by using this line instead:
```sh
src-link easymesh /full/path/to/feed/root
```

