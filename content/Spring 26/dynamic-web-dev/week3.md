for this project i wanted to make a web page that the users type in what ever they like , and my web finds the part of the song that matches closest lyrics to what they typed.

first i found [spotify-api](https://developer.spotify.com/documentation/web-api) from the [public-API](https://github.com/public-apis/public-apis?tab=readme-ov-file#music) list. 
i followed through the documentation of spotify, and a help from Claude i was able to get the spotify Web API. [(chat with claude link)](https://claude.ai/share/2f7709eb-050e-4217-86ff-07ec6bfcc9b9)

i was able to get the spotify API but the i was confused because it required the user to log in with their own spotify account. but this is not what i wanted. 
![[Screenshot 2026-02-12 151904.png]]![[Screenshot 2026-02-12 151916.png]]

so i asked claude again and realized that spotify is not the right API to get to achieve my goal. 
this is what claude said:
![[Pasted image 20260212182134.png]]

but then i encountered another issue with this. 
the issue is that this is not able to run the website from github since the Spotify's Client Credentials flow requires your Client Secret, which you **cannot** safely put in browser JavaScript. Everyone would see it.

good news is, i can work around by using apple music for album arts since the iTunes/Apple Music Search API is completely free, needs **no API key**, and returns album art. 


APIs used (both free, no API key needed!): 
- LRCLIB: https://lrclib.net/docs https://openpublicapis.com/api/lrclib
- iTunes Search API: https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/iTuneSearchAPI/ 

This is my wireframe design :
![[KakaoTalk_20260212_232632647.jpg]]


font from google fonts: https://fonts.google.com/selection/embed

and this is my final output:
![[Pasted image 20260212233211.png]]![[Pasted image 20260212233239.png]]![[Pasted image 20260212233253.png]]![[Pasted image 20260212233313.png]]

and the link :
https://hyejoonhwang.github.io/DynamicWebDev/project3/music-search-by-lyrics/
