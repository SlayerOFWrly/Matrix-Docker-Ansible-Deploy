A revisit and general clean-up of the config files as well as some brief documentation added to the configs to help assist you in the process, not a lot but enough to hopefully get you through install and a working server.

These configs after a few small edits should be easy enough to paste into the required directories along with your own SSL certs from your domain, theoretically you should have a server booted in about 15-25minutes tops. A much needed addition for those who would post on forum boards stuck with initial installs due to long and lengthy Github documentations.

FEATURES:

JITSI audio backend via WebRTC and Screen-sharing, though the Screen-sharing is capped at 30fps and below, currently there is no way via the playbook deployment to change the fps cap, compared to a non playbook install in which you can up the required FPS, so please Spantaleev allow us to pass a custom flag in the vars.yml :). Also for added security I added the Auth variables in which I left some default passwords, I strongly suggest you change these to suit your needs.

HINT /addwidget followed by Jitsi voice room link in Element Client to load a widget for one voice room per chat room you setup, just repeat the /addwidget cmd for each room for a new widget

Zero Federation: I have set Federation to false via a flag in the matrix vars.yml. However in doing so meant some features broke, Element Call being one hence the Jitsi backend and not the the native Element Call which uses the Matrix API, which can only be accessed through Federation if I am not mistaken

My reasoning for this is there are thousands of servers with in the fediverse which host CSAM content. (To Quote the GrapheneOS main Dev from Mastodon) "Attackers cycle through endless accounts to raid rooms to harass our team and community members, troll and spam gore or CSAM (Child Sex Abuse Material). Moderation actions taken to counter these raids is the main way large Matrix rooms get bricked." It is a felony/illegal to contain any Abuse material of the sorts.

Element Custom Themes: from those kind in the community to contribute <3, which are imported from a Github repo https://github.com/aaronraimist/element-themes

Caddy: Instead of using Traefik, this is my preferred reverse proxy of choice, simpler solution. This bundles nicely into the MASH Playbook which offers more services for those interested. To use caddy you'll need your Domain SSL Certificates route them (after first install) to /matrix/traefik/ssl

(Also thank you to all who posted their Caddy configs, I don't remember the exact github repos I visted to find the drafts that I adapted to my needs, but you really filled in the gap where support was lacking, you all know who you are though, GG!! <3)

MASH Playbook: Like I just mentioned for other services and or features, such as OwnCast for a Twitch alternative that allows many custom theming and scripting, for user taste, however requires some basic syntax knowledge to make your profile unique. Also other services like data hosting from NextCloud, instead of paying another company data hosting fees, become your own data host, as well as some essentials like WireGuard and AdGuard Home for DNS wide adblocking, for those who are unable to flash CFW to their router :(. There is a full list of services over on the MASH Github which can be found here https://github.com/mother-of-all-self-hosting/mash-playbook/blob/main/docs/supported-services.md

Rate Limiting: This can be adjusted in vars.yml, ranging from messaging timeouts to user for spam, to registration sign-up spam. The spam filter might be a little extreme, I set it low for those who plan to host bigger servers, where idiots become more prevalent, communities, lol. Unfortunately it will not stop the messages completely, instead buffering them after a set period of time via what you define in the .yml

Email Registration: I have since removed this from the setup though the flags still remain in the MASH vars.yml as well as potentially in the matrix vars file. The reasoning was I didn't see it as a net benefit to the overall setup process as for small communities there is really little need other than a little added verification. It was kinda convoluted to setup and required an extra set of eyes to make sure it was running smoothly as email providers are very finicky and only allow emails with certain signing for security reasons due to phishing, malware, etc. However if you are planning a server for business you'll probably want to re-enable and configure this, my guide no longer covers this as I have since forgotten the setup between then and now, i.e I no longer use it.

Final Notes:

I hope you are able to make sense of the install and that this guide provides an easier install than that offered by others, though others are welcome to contribute to the guide with improvements, as such with any updates to 60FPS Screen-sharing along with many other features I may not even be aware of, just post an issue and detail the improvement, provide a proof of concept and I can try and format it somewhere into the guide where applicable, the same goes for if anyone wants to make an install video using my guide, or heck maybe I might make one at some point, that way we can make it as accessible to new or previous users who attempt(ed) an install and had issues. I'm not always the most active on Socials, as I spend a lot of my time debating and trying to learn on the side, but I always make sure to check back when I feel it's right, like today to update the old guide with a better formatted version, without all the fluff and nonsense :).

Closing Thoughts:

Overall this has been a mini project I've gone to and from for awhile now, it's been both frustrating but fun to learn, it's taught me a fair bit and for that I am grateful. I recently put it back together after sometime as there has been a growing need/demand for open source self hosting due to many local laws surrounding surveillance, chat control bills, and overall fascism from the state when it comes to your freedom of speech as a whole. As such it becomes essential in the meantime to provide places where one can speak freely and openly without the fear of persecution from ones government just because your Ideology opposes theirs, regardless of political spectrum, you do not persecute those who speak their mind under "thought crimes". As many say the internet had become the "Last Bastion" of freedom for many, to which I call that a problem, escapism is not a solution, that's complacency. Regardless, this ain't meant to be a political post, rather an install guide XD.

Take care of yourself <3
