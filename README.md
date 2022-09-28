## Introduction
![Quick Guides UI](docs/guides.png)

The Quick Guides are documents that contain sets of instructions, commands, and comments. Users can use built-in guides in order to gain an understanding of a certain Redis subject, which accelerates the adoption of Redis features among the Redis community.

The content of the Quick Guides can be updated independently without a need to update the entire application.

This document provides an overview of the Quick Guides structure, its elements and contains instructions, recommendations, and best practices for updating the content of Quick Guides.



## Navigation

1. [Structure](#Structure)
2. [Pages](#Pages)
3. [Autoupdate Flow](#Autoupdate)
4. [Development Flow](#Development)

## Structure
Quick Guides allows you to render recursive objects, such as a file directory.

On the root level of guides folder, we have `guides.json` and all necessary static files (markdowns, images, etc.)

The content of this area is generated based on Nodes specified inside `guides.json`.
This JSON file is described as a simple [Object](https://javascript.info/object) (Hash map), where key is a **string** and value is a **Node** (`Record<string, Node>`). Each Node requires a `label`,`type` and a unique `id` (all available properties are described in the table below).

| Prop                 | Type                                    | Description |
| -------------------- | --------------------------------------- | ----------- |
| id (**required**)    | string                                  |             |
| type (**required**)  | "group", "internal-link", "code-button" |             |
| label (**required**) | string                                  | Label that will be displayed on UI for Node |
| children             | Record<string, Node>                    | Use only for "group" type |
| args                 | Record<string, any>                     | A special set of parameters required by a certain type of node (each type has its own)             |

A Node can be represented by various UI components and is specified by `type` prop. Supported types are listed below.

| Type            	| Description                                                                         	| Args                                                                                                                                                                         	|
|-----------------	|-------------------------------------------------------------------------------------	|--------------	|
| **"internal-link"** 	| A link that opens a page inside the Enablement Area <br> (e.g. link to guide page).   | - "path" (required, string) - relative file path                                                                                                              |
| **"code-button"**   	| A button that inserts content into the Code Editor. | - "path" (required, string) - relative file path                                                                                                                             	|
| **"group"**         	| Grouping several nodes into one directory/folder. <br> Allows you to create nested lists | - "initialIsOpen" (boolean) - The group will start in the open state (default value - false)	|

> _**!Note.** All **"internal-link"** nodes located in the same **"group"** will be connected to each other. And using pagination (created dynamically) you can go directly from one page to another. Therefore, it is best to combine elements into groups related to the same topic._

### Example with "Document" group and two guides inside it
```json
{
  "document": {
    "type": "group",
    "id": "document",
    "label": "Document",
    "args": {
      "initialIsOpen": true
    },
    "children": {
      "introduction": {
        "type": "internal-link",
        "id": "introduction",
        "label": "Introduction",
        "args": {
          "path": "/quick-guides/document/introduction.md"
        }
      },
      "learn-more": {
        "type": "internal-link",
        "id": "learn-more",
        "label": "Learn More",
        "args": {
          "path": "/quick-guides/document/learn-more.md"
        }
      }
    }
  }
}
```
## Pages
By using **"internal-link"** node we can open some [Markdown Guides](https://www.markdownguide.org/) located in guides folder.

These guides may contain the following elements:
* Plain text
* [Basic Markdown Syntax](https://www.markdownguide.org/basic-syntax/)
* [Extended Markdown Syntax](https://www.markdownguide.org/extended-syntax/)
* [HTML tags](https://www.markdownguide.org/basic-syntax/#html)
* Images
* Custom Syntax

### Images
Basic Markdown syntax provides the ability to render images. ([Image Markdown Syntax](https://www.markdownguide.org/basic-syntax/#images-1))

You can use the absolute path
```
![RedisInsight Browser screenshot](https://github.com/RedisInsight/RedisInsight/blob/main/.github/redisinsight_browser.png)
```
or relative path to image inside the guides folder
```
![RedisInsight Browser screenshot](/_images/aggregations.png)
```

#### Redis Code block
![Redis Code block](docs/guides-redis-code-block.png)

###### Manual-execute button
A button that inserts Redis commands in the Editor. The syntax is almost the same as for the [Fenced Code Block](https://www.markdownguide.org/extended-syntax/#fenced-code-blocks), 
the only difference is that you must specify `redis` as language and the label next to it (`Create` in the example below).
````
 ```redis Create
    // Let's add three documents as Hashes.
    // Each document represents a building permit.

    HSET permit:1 "description" "To reconstruct a single detached house with a front covered veranda." "construction_value" 42000 "building_type" "single detached house" "address_city" "Lisbon" "work_type" "demolition,reconstruction" "permit_timestamp" "1602156878" "address_street" "R. Da Palma" "location" "38.717746, -9.135839"

    HSET permit:2 "description" "To construct a loft" "construction_value" 53000 "building_type" "apartment" "address_city" "Porto" "work_type" "construction" "permit_timestamp" "1556546400" "address_street" "Rua da Boavista" "location" "41.155854, -8.616721"

    HSET permit:3 "description" "New house build" "construction_value" 260000 "building_type" "house" "address_city" "Lagos" "work_type" "construction;design" "permit_timestamp" "1612947600" "address_street" "R. Antonio Gedeao" "location" "37.114864, -8.668521"

 ```
````

###### Auto-execute button
![img.png](docs/guides-redis-auto-code-block.png)

A button that automatically executes the Redis commands (without inserting it into the Editor). Such buttons will be displayed with the "Play" icon inside.
The code syntax is the same as for the manually executed buttons, just indicate the `redis-auto` (instead of `redis`) before the button.

````
 ```redis-auto Create
    // Let's add three documents as Hashes.
    // Each document represents a building permit.

    HSET permit:1 "description" "To reconstruct a single detached house with a front covered veranda." "construction_value" 42000 "building_type" "single detached house" "address_city" "Lisbon" "work_type" "demolition,reconstruction" "permit_timestamp" "1602156878" "address_street" "R. Da Palma" "location" "38.717746, -9.135839"

    HSET permit:2 "description" "To construct a loft" "construction_value" 53000 "building_type" "apartment" "address_city" "Porto" "work_type" "construction" "permit_timestamp" "1556546400" "address_street" "Rua da Boavista" "location" "41.155854, -8.616721"

    HSET permit:3 "description" "New house build" "construction_value" 260000 "building_type" "house" "address_city" "Lagos" "work_type" "construction;design" "permit_timestamp" "1612947600" "address_street" "R. Antonio Gedeao" "location" "37.114864, -8.668521"

 ```
````


## Autoupdate
Our application supports the ability to quickly update the static files of the Enablement area so that we can provide users with up-to-date information.

EA guides auto-update flow:
1. Push commit with updates to `/guides` repository branch
2. Approve guides release in CircleCI.
3. Reopen the Redisinsight with a working internet connection. In the background, new files will be downloaded to the user's local home directory:
    * **Mac**: In the `/Users/<your-username>/.redisinsight-v2/guides` directory.
    * **Windows**: In the `C:\Users\<your-username>\.redisinsight-v2\guides` directory.
    * **Linux**: In the `/home/<your-username>/.redisinsight-v2/guides` directory.
4. Open Enablement area on UI

![guides-autoupdate-flow](docs/guides-auto-update-flow.png)

## Development
Let's imagine that we need to add a new group (Streams) with 2 pages (Basics, Getting data).

Step by step implementation would be as follows:
1. First of all, it's best to create new pages locally and make sure that everything looks like it was planned.
2. Clone Guides repository (if you haven't done it before) and go into `<folder to guides repo>/src`
3. Create a new branch from the `main`
4. Add new `streams` folder with `basics.md` and `getting-data.md`. As a result, the folder structure will look something like this.
```
guides
 > quick-guides
   > document
   > streams
     basics.md
     getting-data.md
 guides.json
```
5. Add new nodes inside `guides.json`
```json
{
  "quick-guides": {
    "type": "group",
    "id": "quick-guides",
    "label": "QUICK GUIDES",
    "children": {
      "document": {"id": "document"...},
      "streams": {
        "type": "group",
        "id": "streams",
        "label": "Streams",
        "children": {
          "basics": {
            "type": "internal-link",
            "id": "basics",
            "label": "Basics",
            "args": {
              "path": "/quick-guides/streams/basics.md"
            }
          },
          "get-data": {
            "type": "internal-link",
            "id": "getting-data",
            "label": "Get Data",
            "args": {
              "path": "/quick-guides/streams/getting-data.md"
            }
          }
        }
      }
    }
  }
}

```
> _**!Note.** Markdown file name should have the same value as  Node `id` to properly create pagination. _
6. Fill markdowns with content.

`basics.md`
````
For the goal of understanding what Redis Streams are and how to use them, we will ignore all the advanced features, and instead focus on the data structure itself, in terms of commands used to manipulate and access it.
This is, basically, the part that is common to most of the other Redis data types, like Lists, Sets, Sorted Sets and so forth.
However, note that lists also have an optional more complex blocking API, exported by commands like BLPOP and similar.
So streams are not much different than lists in this regard, it's just that the additional API is more complex and more powerful.

Because Streams are an append-only data structure, the fundamental write command, called **XADD**, appends a new entry into the specified stream.
A stream entry is not just a string but is instead composed of one or multiple field-value pairs.
This way, each entry of a stream is already structured, like an append-only file written in CSV format where multiple separated fields are present in each line.

```redis Create
XADD mystream * sensor-id 1234 temperature 19.8
```

````

`getting-data.md`
````
Querying by range: **XRANGE** and **XREVRANGE*
To query the stream by the range we are only required to specify two IDs, _start_ and _end_.
The range returned will include the elements having start or end as ID, so the range is inclusive. The two special IDs - and + respectively mean the smallest and the greatest ID possible.

```redis XRANGE
XRANGE mystream - +
```

```redis XREVRANGE
XREVRANGE mystream + - COUNT 1
```


````
7. Open **RedisInsight** application with `GUIDES_DEV_PATH` environment variable `GUIDES_DEV_PATH='<path to Guides repo>/src' ./<path to RedisInsight binary>`

   Example on Linux: `GUIDES_DEV_PATH='/home/user/projects/Guides/src' ./home/user/programms/RedisInsight.AppImage`
8. Make sure that everything looks fine.

![guides-gif](docs/guides.gif)

9. After that, just commit and push, and then create Pull Request to the main branch. (Release flow described in the [Autoupdate Flow](#Autoupdate) section).


