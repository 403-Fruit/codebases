# Codebases

This is collection of some big project's codebases built with Reactjs that direct downloaded from some websites. They're good for reading and studying, but not recommended for using in product.

All of codebases in this repo should be reviewed by our team, to ensure they are good for users to reference.

## How we get the source code?
This picture show how we get the source code.

<img width="1293" alt="Screen Shot 2021-09-24 at 21 20 37" src="https://user-images.githubusercontent.com/13061304/134693661-e6a53170-505f-44b9-bef0-3c181d6a5d18.png">


See the ["Automate saving source code files"](#Automate) section for an easy way to export the source-mapped files

## Index

**Notes:**
* Before you submit a PR to add a codebase to this repo, you must add it to this index
* Try to keep those items bellow in alphabet sort
* Named template: NAME (URL) - CATEGORIES / ADDED_BY, ADDED_AT. eg: Zappos (https://www.zappos.com/) - eCommerce / Cong Nguyen, 2021/09/20

### A
* Anchor (https://anchor.fm/) - Audio / Cong Nguyen, 2021/09/20

---
### B
* Bestbuy (https://bestbuy.ca) - eCommerce / Cong Nguyen, 2021/09/20

---
### C
* Coursera (https://coursera.org) - education / Cong Nguyen, 2021/09/22
* Calibre (https://calibreapp.com/) - Site monitoring / Nguy Thang, 2021/09/24

---
### D

---
### E
* Elastic (https://cloud.elastic.co) - Searching / Cong Nguyen, 2021/09/20
* Ello (https://ello.co) - Social / Cong Nguyen, 2021/09/23

---
### F

---
### G
* Gatsby Homepage (https://www.gatsbyjs.com) - Landing Page / [Khoa Huynh](https://github.com/htdangkhoa), 2021/09/21
* Glitch (https://glitch.com) - Codesandbox / Nguy Thang, 2021/09/23

---
### H
* Hubspot (https://app.hubspot.com) - CRM / Cong Nguyen, 2021/09/23

---
### I
* Intercom (https://intercom.com) - Messaging, Nextjs, SSR / Cong Nguyen, 2021/09/20
* Invision App dashboard (https://invisionapp.com) - Utilities, Marketing tool / Cong Nguyen, 2021/09/23

---
### J

---
### K

---
### L

---
### M

---
### N

---
### O
* Microsoft Outlook (https://outlook.live.com) - Utilities, Email / Cong Nguyen, 2021/09/23

---
### P
* Pipedrive dashboard (https://pipedrive.com) - CRM / Cong Nguyen, 2021/09/23
* Postman Web (https://web.postman.co) - electron / Cong Nguyen, 2021/09/20

---
### Q

---
### R

---
### S
* SendBird (https://dashboard.sendbird.com) - Messaging / Cong Nguyen, 2021/09/20
* Skillshare (https://skillshare.com) - Education / Cong Nguyen, 2021/09/22
* Similarweb (https://pro.similarweb.com) - Ranking / Cong Nguyen, 2021/09/23
* Stackshare (https://stackshare.io) - QA / Cong Nguyen, 2021/09/24

---
### T
* Telegram (https://web.telegram.org) - Messaging / Cong Nguyen, 2021/09/20
* Toptal Homepage (https://toptal.com) - freelance / Nguy Thang, 2021/09/20
* Tribe Platform (https://tribeplatform.com) - Social / Cong Nguyen, 2021/09/23

---
### U
* Auth Udacity - (https://auth.udacity.com/) - education / Nguy Thang, 2021/09/20
* Classroom Udacity (https://classroom.udacity.com) - education / Nguy Thang, 2021/09/20

---
### V

---
### W

---
### X

---
### Y

---
### Z
* Zappos (https://www.zappos.com/) - eCommerce / Cong Nguyen, 2021/09/20


## How to contribute
Contributions are always welcome, no matter how large or small!

_(document how to make a PR is in construction)_

## Community
We have a facebook group for update and talking about these codebases.
Unfortunately, only Vietnamese is supported now. [Click here](https://www.facebook.com/groups/codebases) to visit our group.  


<a name="Automate"></a>
## Automate saving source code files
Chrome Devtools does not allow you to save entire folders from the sources tab, but you can automate this process by opening a Devtools window for the Devtools window itself. To do this: 
- Navigate to `chrome://inspect/#pages` in your browser.  
- Look for a title like "DevTools - *example.com*" where *example.com* is the target site, and click inspect to open a new Devtools window.  
- In the new devtools window, use the following snippet to generate a JSON of all the source-mapped files from the target site.  
    ```javascript
    let srcFiles = UI.panels.sources.workspace.uiSourceCodesForProjectType("network");
    
    await Promise.all(srcFiles.map(async (f) => {
        let file = await f.requestContent();
        if (file.content === null) file.content = file.error || "Error loading file";
    
        let name = f.displayName();
        if (name === "(index)") name = "index.html";
        if (/\?/.test(name)) name = name.replace(/\?.*/, "");
    
        let originName = f.originInternal.replace(/^(https|chrome|chrome-untrusted)?:\/\//, "").replace(/:?[\/\\]+$/g, "").replace(/[\/\\]+/g, "_");
        return {
            name: f.displayName(),
            path: f.parentURLInternal.replace(f.originInternal, ""),
            content: file.content,
            origin: originName
        };
    }));
    ```  
- Copy the JSON output from the previous snippet  
- Using NodeJS, run the following snippet after modifying it with the JSON copied in the previous step.  
    ```javascript
    const fs = require("fs");
    const path = require("path");
    
    const debugMode = false; // If true, dont execute any file operations
    
    let srcInputFile = process.argv[2];
    if (!srcInputFile) 
    {
        console.error("Usage: devtools-export.js <input-file>");
        process.exit(1);
    }
    
    if (!fs.existsSync(srcInputFile))
    {
        if (!/[\/\\]/.test(srcInputFile) && fs.existsSync(path.join(process.env.USERPROFILE, "Downloads", srcInputFile)))
        {
            srcInputFile = path.join(process.env.USERPROFILE, "Downloads", srcInputFile);
        }
        else
        {
            console.error(`File '${srcInputFile}' not found.`);
            process.exit(1);
        }
    }
    
    
    if (debugMode)
    {
        fs.writeFileSync = (...args) => {
            console.log("\x1B[31mwriteFile \x1B[90m~>\x1B[39m", path.resolve(args[0]), "|", args[1].length, "\x1B[33mbytes\x1B[39m");
        };
        
        fs.mkdirSync = (...args) => {
            console.log("\x1B[32mmkdir     \x1B[90m~>\x1B[39m", path.resolve(args[0]));
        };
    }
    const outDir = "./output";
    if (!fs.existsSync(outDir)) fs.mkdirSync(outDir);
    
    const srcFiles = JSON.parse(fs.readFileSync(srcInputFile, "utf8"));
    
    
    srcFiles.forEach((file) => {
        file.origin = file.origin.replace(/[\\\/\?\|:\*<>]/g, "");
        file.name = file.name.replace(/\?.*$/g, "").replace(/[\\\/\?\|:\*<>]/g, "");
        const destDir = path.join(outDir, file.origin, file.path).replace(/[\?\|:\*<>]/g, "");
    
        if (!fs.existsSync(destDir))
        {
            fs.mkdirSync(destDir, { recursive: true });
        }
        
        if (file.name === "(index)")
        {
            file.name = "index.html";
        }
        try {
            fs.writeFileSync(path.join(destDir, file.name), file.isEncoded ? atob(file.content) : file.content);
        } catch(e) {
            console.warn(`Failed writing file '${path.join(destDir, file.name)}' -`, e.message);
        }
    });
    ```
