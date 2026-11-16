I contributed to the open-source All the Places project by building support for Isuzu UTE dealerships in Australia. I opened a pull request that uses Isuzu's official dealer data to surface 165 locations.

Finding the right issue took a while. My work is in infrastructure, Linux, cloud and automation, and most beginner issues I came across were far from that. This one stood out because it meant digging into how a real data source worked, automating the collection, and finding my way around a codebase I had never seen before.

The hardest part was the data itself. Dealer records weren't always laid out the same way, and some status labels didn't mean what I first assumed. I learned to check each assumption against the actual data instead of trusting it.

My local tests passed, and the project's own automated check independently returned all 165 locations.

My biggest takeaway was that understanding a project's conventions and validating assumptions carefully matter just as much as getting the code to run.

https://github.com/alltheplaces/alltheplaces/pull/20576
