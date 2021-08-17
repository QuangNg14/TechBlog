---
title: Intern at seed stage startup Alias
catalog: true
date: 2021-08-15 15:39:31
subtitle:
header-img: "image-4.png"
tags:
readtime:
---

## This is my first time working at a startup and I learnt a lot of professional skills, work ethics and technologies application

This is the main page of the website: [https://alias.co](https://alias.co/). This is a current [podcast page](https://www.notion.so/c2712b92719848f5899ff3fe2530e822) and there is also this cool [chat page](http://chat.alias.co/paul-graham?password=8TAqXioAxA)

### General

Alias is a seed stage startup aggregating content for influencers. The content can be essays, papers, podcasts, tweets or youtube videos. The main focus of the startup right now is to dive deeper and utilize the podcasting tool.

### Technologies that we used

- **Front-end**: We used Typescript and Tailwind CSS.
- **Back-end**: We used GraphQL Hasura, Postgres Database and deploy the Backend on Heroku
- We used Docker to create a container for Hasura to run
- **Deployment**: NextJS and Vercel for the Frontend and Heroku for the Backend
- **Notion:** As a dashboard to communicate and deliver task

We used Vercel auto deployment for each successful commits we made so it is really handy to check the current progress of the page

### Tasks that I have done

There are some big tasks that I have finished for the startup and these tasks really helped me developed as a software engineer. I also understand many important professional work ethics.

1. **Adding "role" for each user**

   Before, if we want to add another admin to the page, we need to manually add in the email to check (inside the code). This is not something that we want to do because as time goes by, the list will be huge and the code will bloat up.

   Thus, I added a role column in each user's table indicating they are **user** or **admin.** I created an enum table and make a **foreign key** link from the role column to the enum table. In this way, the role column now has 2 default values **user** and **admin**

   {% asset_img add_user_role.png %}

2. **Fixed profile images of each influencer**

   Each influencer has a profile that is hand-made (a cartoon profile). This will be automatically displayed in the profile page. However, there are some people who don't have the hand-made avatar yet so we will pull the Twitter avatar in place of that.

   Because all the Twitter avatars have been stored in our database, I only need to make a simple check whether the user has the hand-made avatar or not.

   Here is the Twitter avatar in place of the cartoon avatar:

   {% asset_img add_cartoon_image.png %}

3. **Edit biography and add hyperlinks**

   1. Edit Biography

      If an admin enters each profile pages, they can edit the biography and it will automatically save and update in the database. In order to do this, I used **EditorJS.** This allows me to save the **HTML elements** of the biography and upload the whole HTML element to the database and store as **string**.

      When I pulled the data from the database, I only need to parse it to the right format. This is how it looks:

      {% asset_img bio1.png %}

   2. Add hyperlinks

      We also want admins or influencers to add links to their bio and be able to share it. Thus, I enabled hyperlinks-adding to each text. This is a part of the EditorJS tool. What I did was replace the HTML part of the linked parts to "<a> ... </a>" and saved that back to the database.

      {% asset_img bio2.png %}

4. **Improve page performance**

   This might be the biggest task that I have done. The page was originally really slow so I used Lighthouse analytics (in the Chrome tab of each page) and improved the scores in both the main page and the profile page.

   Here are some things that might worsen the page performance:

   1. **Not properly sized image:**

      This is most probably why the page is slow. Many images are really heavy and it need to be resized to make it lighter. This is an example of a [non-resize image](https://ucarecdn.com/019d4eff-bc50-4823-a96e-e425db881fb4/avichal.jpg) and this is with [resized images](https://ucarecdn.com/019d4eff-bc50-4823-a96e-e425db881fb4/-/resize/x100/-/format/webp/avichal.jpg).

      You can see if we rendered 50 images not properly sized the page would be really slow. So I fixed each image and resized them properly

      I also utilized Next Image. This will automatically optimize image rendering for pages made with NextJS

   2. **Unused imports:**

      This is where code-splitting is needed. I used dynamic imports from Next to minimize unused imports. These dynamic imports can be for searches, modals or pop-ups any components that do not show up right away when we see the page.

   3. **Fonts and icons:**

      When we used fonts like FontAwesome, we typically have to add a line that imports everything from FontAwesome to our app but we might only need to use 3-4 icons. This can make the app run slower. Thus, I replaced the FontAwesome with auto-generated SVGs from **Figma**

      {% asset_img svg_icon.png %}

      We also tried to statically render the homepage in order to better the performance

      {% asset_img static-render.png %}

5. **Add deeplinks for each podcast episode**

   We used Listennotes API and get many podcast links and episodes. However, we want to get the deeplinks for each of the episode to Spotify and Apple Podcast platforms. Thus, I used the Spotify API and Itunes Search tool and create customized deep links for all the podcasts.

   From a Listennote link like [this](https://www.notion.so/529703b5e7224b509b5e53170344b520) , I was able to obtain 2 deeplinks to [Spotify](https://open.spotify.com/episode/5c6kDUMAMwx6u8vdZmzZND) and [Apple Podcast](https://podcasts.apple.com/us/podcast/extra-crunch-live-with-hunter-walk-from-homebrew/id1215439780?i=1000474476715&uo=4) correspondingly to the Listennote link.

   - **Itunes:** Here is an example of an API search for an episode in **Itunes** (Apple Podcast). This API is **not** in the documentation and it took me a long time to figure it out:

     ```jsx
     const fetchApplePodcastDeeplink = async (title: string) => {
       let apple_podcast_deep_link = "";
       try {
         await axios(
           `https://itunes.apple.com/search?term=${encodeURIComponent(
             title
           )}&entity=podcastEpisode`,
           {
             method: "GET",
           }
         )
           .then((data) => {
             if (data.data.results[0]) {
               apple_podcast_deep_link = data.data.results[0].trackViewUrl;
             } else {
               console.log(
                 "The Podcast is not available in Apple Podcast or Itunes"
               );
               return "";
             }
           })
           .catch((err) => console.log(err));
       } catch (error) {
         console.error(error);
       }
       return apple_podcast_deep_link;
     };
     ```

   - **Spotify**: Here is an example of an API call to find the episode by title in Spotify. Spotify requires authentication so it is a bit more complicated to call.

     ```jsx
     const fetchSpotifyPodcastDeeplink = async (title: string) => {
       let spotify_deep_link = "";
       await axios("https://accounts.spotify.com/api/token", {
         headers: {
           "Content-Type": "application/x-www-form-urlencoded",
           Authorization:
             "Basic " +
             Buffer.from(ClientId + ":" + ClientSecret).toString("base64"),
         },
         data: "grant_type=client_credentials",
         method: "POST",
       }).then(async (tokenResponse) => {
         try {
           await axios(
             `https://api.spotify.com/v1/search?q=${encodeURIComponent(
               title
             )}&type=episode&market=US`,
             {
               method: "GET",
               headers: {
                 Authorization: "Bearer " + tokenResponse.data.access_token,
               },
             }
           )
             .then((data) => {
               if (data.data.episodes.items[0]) {
                 spotify_deep_link =
                   data.data.episodes.items[0].external_urls.spotify;
               } else {
                 console.log("The Podcast is not available in Spotify");
                 return "";
               }
             })
             .catch((err: string) => console.log(err));
         } catch (error) {
           console.error(error);
         }
       });
       return spotify_deep_link;
     };
     ```

### How I processed each tasks

1. First, I will have a meeting with the managers to talk and discuss my next ticket (this is what we call a task in the Notion tasks dashboard. Each ticket will have a detailed description of what the current problem is and what should we do next. There are some tickets that have open contribution, which means I can do the task in my way.
2. Then after I was assigned a task, I would create a new branch from the **staging branch** to complete the feature. I would then try to accomplish the feature with as few **iterations** as possible. I would try to research the solutions and see which solution is the most suitable and effective.
3. Usually my task involved both frontend and backend so I will have to modify the database a lot. Since we use **Hasura & PostgreSQL**, we have Hasura migrations which captures the changes made to the database (create a new column, modify a property, delete a column). When I am doing the task, it is important to keep track of the migrations and try to delete duplicate migration (for example, I added column "role" then I deleted column "role" → then I repeat that process. This will create **duplicates** and it is hard to keep track so either I have to control the migrations or I have to **stash** the migrations later
4. After I have done my task, I have to submit a pull request for the managers to review. We will discuss back and forth about further changes in the pull request typically for 3-4 days. And after reviewing and making changes, the pull request is ready to be merged. I will merge the pull request into **staging branch.** After careful testing, it will be merged into production (**master branch**).
5. However, merging the pull request is not enough. If I made changes to the database, I will have to **migrate** the hasura migrations from my local database to **staging database** and the **master database.** Only after that, a task is officially done and in production

That's all the experience I have in my first seed stage start up. This startup really helped prepare me for software engineering work and I am happy to share my knowledge with you guys.
