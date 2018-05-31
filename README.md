# The Connected Learning Profile (CL Profile)

This profile describes the xAPI (https://github.com/adlnet/xAPI-Spec) verbs, activities, and relationships between users of various social media networks in a format that enables Learning Analytics products to be applied interoperably across all platforms. It is a reimplementation of the Connected Learning Recipe (https://github.com/kirstykitto/CLRecipe) that has been rewritten to meet the new xAPI Profiles Specification (https://github.com/adlnet/xapi-profiles).

The starter template at  https://github.com/adlnet/xapi-authored-profiles/blob/master/starter-template.jsonld was used in writing the Profile.
Where possible, the vocabulary used in the profile was obtained from http://xapi.vocab.pub.

## Modeling Social Media Interaction as an xAPI statement

Though social media platforms use different terminology, a common model of interaction exists (see figure below). The model involves:
* A user can create a post.
The post could be a short microblog post or a full article.
* A user can optionally including tag/s, hashtag/s or mention other users when creating the post.
* Other users can share a post (i.e. for their followers to view) and optionally including a comment.
* Other users can replying to the original post.
* Other users can rate the original post.

![Social Media Model](https://github.com/kirstykitto/CLRecipe/blob/master/socialmediamodel.png)

The Profile prescribes that:
* A common set of verbs and objects must be used to represent activity across all social media platforms. Terminology used by specific platforms like Twitter and Facebook is mapped to a standard set of object and verbs (e.g. A 'Tweet' and Facebook Timeline post will both be mapped to the Note object; Twitter 'Favorite' and Facebook 'Like' will be represented by the 'Like' verb).
* A distinction between Short Microblogging posts (i.e. using the Note object) and larger text articles (i.e. using the Article object for pages and blog posts). This gives an indication of the size of the text included and is useful for content analysis (analytics).
* The social media platform must be specified using the *context.Platform* property. The inclusion of the platform (e.g. Twitter, Facebook, Blog, etc) will allow statement to be queried from the LRS based on the originating platform.  
* Tags, hashtags and @mentions that are included in a post must all be included in a single xAPI statement that represents the original post. There were two options to achieve this namely using  *context.contextActivities.Other* or *context.contextActivities.Category*. *context.contextActivities.Other* was selected because tags, hashtags and mentions represent folksonomies created by the user. *context.contextActivities.Category* is more suitable for use by custom taxonomies that are created at a system level for enhanced reporting (e.g. Cognitive Presence tagging, Sentiment Analysis, etc).
* Shares and likes must include a reference back to the object being shared or liked using *contect.contextActivities.Parent*. If the referenced id does not exist in the LRS it can be inserted using a substatement to represent the object. *contect.contextActivities.Parent* was chosen over using a *statementref* because because *contect.contextActivities.Parent* represents a direct parent-child relationship which required for social network analysis.
* Imported social media harvested from a group discussion area (i.e. Google+ Group or Facebook Group) must include *context.Team*.

Option additions to the recipe to facilitate reporting by course and instructor include:
* Imported social media should be associated with a course using *context.contextActivities.Grouping*.
* Imported social media should be associated with an instructor using *context.Instructor*.

### Activities

Activities are items that are created on social media platforms. Blog posts, tweets, homepage posts and curated media are all example objects. The CL Profile makes a distinction between short Microblogging posts (i.e. using the Note object) and larger text articles (i.e. using the Article object).

| Activity  | Description | Source |
| ------------- | ------------- | ------------- |
| Note  | Represents a short-form text message. This object is intended primarily for use in "micro-blogging" scenarios and in systems where users are invited to publish short, often plain-text messages.  | [Activity Stream Schema](http://activitystrea.ms/head/activity-schema.html#note) |
| Article | Represents objects such as news articles, knowledge base entries, or other similar construct. Such objects generally consist of paragraphs of text, in some cases incorporating embedded media such as photos and inline hyperlinks to other resources.  | [Activity Stream Schema](http://activitystrea.ms/head/activity-schema.html#article) |
| Comment | Represents a textual response to another object. Objects of this type MAY contain an additional inReplyTo property whose value is an Array of one or more other Activity Stream Objects for which the object is to be considered a response. | [Activity Stream Schema](http://activitystrea.ms/head/activity-schema.html#comment) |
| Collection | Represents a generic collection of objects of any type.  | [Activity Stream Schema](http://activitystrea.ms/head/activity-schema.html#collection) |
| Audio | Represents audio content of any kind.  | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/audio) |
| File | Represents any form of document or file. | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/file) |
| Image | Represents a graphical image.  | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/image) |
| Video | Represents video content of any kind. | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/video) |
| Link | The URL to another resource.   | [Activity Stream Schema](http://adlnet.gov/expapi/activities/link) |
| Bookmark | Represents a pointer to some URL.   | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/bookmark) |


The table below show how social media activities from different social media platforms map to the Activities chosen for use by the CL Profile.

| Platforms  | Note | Article | Comment | Collection |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| Twitter - Tweet | Yes |  |  |  |
| Facebook -  | Yes (Post) | Yes (Page)  |  | |
| Blog Post | | Yes  | Yes | |
| Pinterest | |  | Yes | Yes |

### Verbs

Verbs are actions that are performed on an object. Most of the verbs used in the CL Profile are from the [Activity Stream API](http://activitystrea.ms/head/activity-schema.html#verbs)

| Verb  | Description | Source |
| ------------- | ------------- | ------------- |
| Create  | Indicates that the actor has created the object.  | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/create) |
| Like  |  Indicates that the actor marked the object as an item of special interest. The "like" verb is considered to be an alias of "favorite". The two verb are semantically identical. | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/like) |
| Share  | Indicates that the actor has called out the object to readers. In most cases, the actor did not create the object being shared, but is instead drawing attention to it.  | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/share) |
| Tag  | Activity generally used in the "other" or "grouping" Context Activities lists to mark a statement as being related to a particular subject area. Implemented as a one word identifier used for search filtering or tag cloud generation. Includes hashtagging for tweets. | [TINCAN API](http://activitystrea.ms/schema/1.0/tag) |
| Rate  | Action of giving a rating to an object. In general the rating should be included in the Result with a Score object. There is an extension to include the 'raw' value as well as 'min' and 'max' range indicators. | [TINCAN API](http://id.tincanapi.com/verb/rated) & [TINCAN Quality Rating Extension](http://id.tincanapi.com/extension/quality-rating) |
| Add  | Indicates that the actor has added the object to the target. For instance, adding a photo to an album.  | [Activity Stream Schema](http://activitystrea.ms/schema/1.0/add) |

The table below show how actions from different social media platforms map to the Verbs chosen for use in the CL Profile.

|   | Create | Like | Share | Tag | Rate | Comment | Add |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| Facebook | Post | Like | Share | Tag | - | Reply | - |
| Google+ | Post | Like | Share | Tag | - | Reply | - |
| Twitter | Tweet | Favorite | Retweet | Hashtag | - | - |
| Blog | Post | - | - | Tag | Rate | Comment | - |
| Pinterest | Board | Like | Share | - | - | - | Pin |
