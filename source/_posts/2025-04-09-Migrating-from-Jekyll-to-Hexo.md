---
title: Migrating from Jekyll to Hexo
date: 2025-04-09 23:29:38
tags: [Hexo]
toc: true
---



Been using Jekyll for GitHub Pages since the beginning, but after switching out my old laptop, I never managed to get the local environment set up properly. Then the theme I was using became unmaintained—and not being able to tweak the layout locally when everything started breaking was seriously frustrating.

Recently came across **Hexo**—found it pretty similar to use and super easy to host locally. What really impressed me was that it only took about 2 hours to migrate my old project to Hexo, and honestly, half of that time was just me browsing for a theme I liked.

## The migration

#### Step 1: Create a new Hexo project

As stated on the official [Hexo website](https://hexo.io/docs/setup), you only need **Git**, **Node.js**, and **npm** installed locally. Once that’s set up, just follow the instructions:

```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

After running these commands, you should see a few files and folders created in the project directory.

#### Step 2: Configuration

Just like in Jekyll, configurations in Hexo are defined in the `_config.yml` file. You can modify the settings there as needed.

There are two key configurations to pay attention to:

1. **`new_post_name`**:
    By default, Hexo generates new post filenames based on the input title, like `title.md`. To match Jekyll’s convention more closely, you can update it to include the date:

   ```yaml
   new_post_name: ':year-:month-:day-:title.md'
   ```

2. **Asset folder management**:
    Unlike Jekyll, Hexo offers built-in asset folder handling. There are two ways to manage assets:

   * **Global asset folder** (similar to Jekyll):
      You can manually organize files however you like. In this case, no changes to `_config.yml` are needed.

   - **Post-specific asset folders**:
      A more structured approach, where each post has its own associated asset folder. To enable this, update your config as follows:

     ```yaml
     post_asset_folder: true
     marked:
       prependRoot: true
       postAsset: true
     ```

     If you’ve updated `new_post_name` as shown above, make sure to also update the `permalink` setting to ensure asset paths are correctly resolved:

     ```yaml
     permalink: ':year/:month/:day/:title/'
     ```



#### Step 3: Move data

With the configurations set, we’re good to go. The nice thing is that Hexo’s file structure is quite similar to Jekyll’s—you just need to move everything inside your old `_posts` folder into the `source/_posts` folder in your Hexo project.

For asset folder, as mentioned earlier, if you prefer to organize files manually, simply copy the asset folders into the `source` directory. Everything under the `source` folder will be copied to the `public` folder when generating static pages. This means you can reference images directly using syntax like `![](/images/image.jpg)`. It won’t show the image immediately in the markdown editor, but it will point to the correct location once the project is built.

If you’re using `post_asset_folder = true` to manage asset folders, there’s a bit more setup. Run the following command to create asset folders for your existing posts and then move the corresponding files into them:

```bash
mkdir $(basename -a -s .md ./*.md)
```

Also, make sure to simplify your image paths—you only need something like `![](image.jpg)` inside the post.

#### Step 4: Choose a theme

One last thing, choose a theme for the website. There are way more themes than I expected, so I ended up picking one at random: [Icarus](https://ppoffice.github.io/hexo-theme-icarus/). Also, switching between themes is super easy—just follow the instructions, and it shouldn’t take more than five minutes.

#### Step 5: Deploy

The website offers two deployment options. The more common approach is to use [One-Command Deployment](https://hexo.io/docs/one-command-deployment). In this case, you don’t need to update your repository manually—the deployer will upload all the static files (HTML, CSS, etc.) to GitHub once the build is complete.
**Note:** If you choose this method, the repository will only contain the generated static files after deployment.



The other way to deploy is to commit all your local files (make sure `public/` is added to your `.gitignore`), then use a GitHub Action to build and deploy the website.

Voila! The migration is officially done.

