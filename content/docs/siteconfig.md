---
title: "SiteConfig Reference"
slug: "siteconfig-setup"
description: "Reference for SiteConfig, the JSON file that connects SKIES to a community service: endpoints, sign-in, capabilities and field mappings."
weight: 6
toc: true
---

SiteConfig describes how SKIES talks to a community service: where it lives, how users sign in, which features it supports, and what its API's paths and JSON keys are called. It's for people deploying SKIES for an organization. App users never need to touch it.

> SiteConfig is public build metadata. Anyone with the app can extract it. Put only endpoints and public OAuth identifiers in it, never client secrets, tokens, passwords or signing material.

## How it's loaded

- **Bundled.** A build embeds one SiteConfig. Without one, the app runs on a built-in demo configuration with sample data. Production builds require a valid file.
- **Imported.** If the build allows it, a SiteConfig file can be imported at runtime. In `diagnostic` mode the app only validates and previews the file. In `enabled` mode it can also activate it after you confirm. Import is off by default.

The app rejects a file that fails validation and keeps the current configuration.

## Validation rules

- The schema is strict. Every key shown below is required, and unknown keys are rejected.
- `schemaVersion` must be `1`.
- Every URL in `connection` must be a plain `https://` URL.
- Imported files may not point at `example.com` placeholder hosts.
- API paths are relative segments, such as `posts` or `v2/users`. They can't start with `/` or contain `..`, `?` or `#`.
- If `authMode` is `required`, or any sign-in value is set, then `tenantID`, `clientID`, `redirectURI` and `scopes` must all be set. `scopes` must include `openid` and at least one API scope, and the `redirectURI` scheme must be one the build registers. A build registers the scheme of its bundled config, so an imported config must use the same scheme.

## Structure

```json
{
  "schemaVersion": 1,
  "capabilities": { },
  "links": { },
  "api": { "paths": {}, "queryKeys": {}, "bodyKeys": {}, "wrapperKeys": {}, "authPolicies": {} },
  "fields": { "post": {}, "activity": {}, "activityParticipant": {}, "user": {}, "comment": {} },
  "assets": { "activityAssetIdentifierStrategy": "postIdOrActivityId" },
  "textFormatting": { "outgoingLineBreak": "crlf" },
  "connection": { }
}
```

## connection

Where the service lives and how to sign in.

| Key | Description |
| --- | --- |
| `apiBaseURL` | Root of the REST API. `api.paths` are appended to it. |
| `assetsBaseUrl` | Base URL for post and activity images and attachments. |
| `avatarOrgBaseURL` | Base URL for profile photos. |
| `webPortalURL` | The web portal, combined with `links.webPortalPath` for shareable links. |
| `profilerURL` | External profiler, used when `supportsProfiler` is on. |
| `tenantID` | Microsoft Entra tenant ID, or `null` when sign-in is off. |
| `clientID` | App registration client ID, or `null`. |
| `redirectURI` | Redirect URI registered for the client, or `null`. |
| `scopes` | OAuth scopes, for example `["openid", "profile", "offline_access", "api://<client>/<scope>"]`. |
| `authProviderDisplayName` | Name shown on the sign-in button, such as `"Microsoft"`. |

## capabilities

| Key | Type | Description |
| --- | --- | --- |
| `authMode` | `none`, `optional`, `required` | Whether users can't, may or must sign in. |
| `supportsUserDirectory` | boolean | People search and profiles. |
| `supportsComments` | boolean | Reading and writing comments. |
| `supportsLikes` | boolean | Like counts and liking. |
| `supportsActivityRegistration` | boolean | Signing up for activities. |
| `supportsProfiler` | boolean | Profiler link on profiles. |
| `birthYearHiddenJobTitles` | string[] | Job titles whose birth year is hidden. Matching is exact. |
| `meaninglessDietaryValues` | string[] | Dietary values treated as "none", such as `"n/a"`. Matching ignores case. |

## links

Builds shareable portal links as `webPortalURL + webPortalPath?{resourceQueryKey}={…ResourceID}&{…IdentifierQueryKey}={id}`.

| Key | Description |
| --- | --- |
| `webPortalPath` | Path appended to `webPortalURL`. |
| `resourceQueryKey` | Query key naming the resource type. |
| `postIdentifierQueryKey`, `activityIdentifierQueryKey`, `profileUsernameQueryKey` | Query keys for the post ID, activity ID and username. |
| `postResourceID`, `activityResourceID`, `userResourceID` | Values of `resourceQueryKey` for each type. |
| `postExtraQueryItems`, `activityExtraQueryItems`, `profileExtraQueryItems` | Extra `{ "name", "value" }` query items. Use `[]` for none. |

## api

### paths

Relative path segments for `posts`, `activities`, `me` (current user), `users`, `user`, `comments`, `participants`, `likes` (liking posts and activities) and `like` (liking a comment, appended to its comment path).

### queryKeys

Query parameter names for `page`, `size`, `department`, `geography`, `name`, `mobile`, `userName`, `subscribed`, `registrationAnswer`, `guestCount`, `commentParentID`.

### bodyKeys

Request and response body keys: `comment` (comment text), `liked` (like state) and `count` (like count).

### wrapperKeys

Response keys: `items` (list results), `users` (directory results), `totalCount`.

### authPolicies

What each request does when the user isn't signed in. A token is always sent when one is available. With `none` or `optional` the request goes ahead without one, and with `required` it fails. Keys: `posts`, `postDetails`, `postComments`, `commentCreation`, `commentDeletion`, `activities`, `activityDetails`, `activityComments`, `activityParticipants`, `activityRegistration`, `currentUser`, `users`, `userDetails`, `likes`, `fileDownloads`.

## fields

Maps each app field to the JSON key your API uses, per model: `post`, `activity`, `activityParticipant`, `user` and `comment`. Every app field must be mapped. `activity.organizerPhotoFallbacks` is the only array: other keys to try, in order, when the organizer photo is missing. See the full example below for every field.

## assets and textFormatting

| Key | Values | Description |
| --- | --- | --- |
| `assets.activityAssetIdentifierStrategy` | `postIdOrActivityId`, `activityId` | Which ID activity image URLs are built from. |
| `textFormatting.outgoingLineBreak` | `crlf`, `lf` | Line breaks in text the app sends, such as comments. |

## Full example

This is the demo configuration that ships with the app. Replace every value with your service's.

```json
{
  "schemaVersion": 1,
  "capabilities": {
    "authMode": "optional",
    "supportsUserDirectory": true,
    "supportsComments": true,
    "supportsLikes": true,
    "supportsActivityRegistration": true,
    "supportsProfiler": true,
    "birthYearHiddenJobTitles": ["consultant"],
    "meaninglessDietaryValues": ["nothing", "none", "n/a", "no", "nil"]
  },
  "links": {
    "webPortalPath": "/portal",
    "resourceQueryKey": "resourceId",
    "postIdentifierQueryKey": "postId",
    "activityIdentifierQueryKey": "activityId",
    "profileUsernameQueryKey": "userName",
    "postResourceID": "post",
    "activityResourceID": "activity",
    "userResourceID": "user",
    "postExtraQueryItems": [],
    "activityExtraQueryItems": [{ "name": "alt", "value": "1" }],
    "profileExtraQueryItems": [{ "name": "alt", "value": "1" }]
  },
  "api": {
    "paths": {
      "posts": "posts",
      "activities": "activities",
      "me": "current-user",
      "users": "users",
      "user": "user",
      "comments": "comments",
      "likes": "reactions",
      "like": "reaction",
      "participants": "participants"
    },
    "queryKeys": {
      "page": "page",
      "size": "size",
      "department": "department",
      "geography": "location",
      "name": "name",
      "mobile": "phone",
      "userName": "userName",
      "subscribed": "subscribed",
      "registrationAnswer": "answer",
      "guestCount": "guestCount",
      "commentParentID": "parentId"
    },
    "bodyKeys": { "comment": "body", "liked": "isLiked", "count": "itemCount" },
    "wrapperKeys": { "items": "items", "users": "users", "totalCount": "totalCount" },
    "authPolicies": {
      "posts": "optional",
      "postDetails": "optional",
      "postComments": "optional",
      "commentCreation": "required",
      "activities": "optional",
      "activityDetails": "optional",
      "activityComments": "optional",
      "activityParticipants": "optional",
      "activityRegistration": "required",
      "currentUser": "required",
      "users": "optional",
      "userDetails": "optional",
      "likes": "required",
      "commentDeletion": "required",
      "fileDownloads": "optional"
    }
  },
  "fields": {
    "post": {
      "id": "id",
      "title": "title",
      "body": "body",
      "imageUrl": "imageUrl",
      "created": "createdAt",
      "city": "city",
      "country": "country",
      "companyName": "organizationName",
      "departmentId": "departmentId",
      "departmentName": "departmentName",
      "companyId": "organizationId",
      "summary": "summary",
      "link": "link",
      "userName": "userName",
      "userPhoto": "userPhotoUrl",
      "pinned": "pinned",
      "hasExtraParticipants": "hasAdditionalParticipants",
      "commentsCount": "commentsCount",
      "likesCount": "likesCount",
      "liked": "liked",
      "global": "isGlobal",
      "activityId": "activityId",
      "activityCommentsCount": "activityCommentsCount",
      "startDate": "startDate",
      "signUpDeadline": "signUpDeadline",
      "totalCapacity": "totalCapacity",
      "subscribed": "subscribed",
      "question": "registrationQuestion",
      "attachment": "attachmentUrl",
      "important": "isImportant"
    },
    "activity": {
      "id": "id",
      "postId": "postId",
      "link": "link",
      "title": "title",
      "body": "body",
      "type": "type",
      "imageUrl": "imageUrl",
      "created": "createdAt",
      "startDate": "startDate",
      "signUpDeadline": "signUpDeadline",
      "cityId": "cityId",
      "cityName": "cityName",
      "countryId": "countryId",
      "countryName": "countryName",
      "departmentId": "departmentId",
      "departmentName": "departmentName",
      "companyId": "organizationId",
      "companyName": "organizationName",
      "organizerUserName": "userName",
      "organizerPhoto": "organizerPhotoUrl",
      "organizerPhotoFallbacks": ["userPhotoUrl"],
      "pinned": "pinned",
      "hasExtraParticipants": "hasAdditionalParticipants",
      "totalCapacity": "totalCapacity",
      "subscribed": "subscribed",
      "commentsCount": "activityCommentsCount",
      "likesCount": "likesCount",
      "liked": "liked",
      "location": "location",
      "tags": "tags",
      "requirements": "requirements",
      "category": "category",
      "question": "registrationQuestion",
      "global": "isGlobal"
    },
    "activityParticipant": {
      "userName": "participantId",
      "signupDate": "signupDate",
      "department": "department",
      "guests": "guestCount",
      "answer": "response",
      "foodRestrictions": "foodRestrictions",
      "photo": "photoUrl"
    },
    "user": {
      "userName": "userName",
      "firstName": "firstName",
      "lastName": "lastName",
      "userPhoto": "userPhotoUrl",
      "workPhone": "workPhone",
      "personalPhone": "personalPhone",
      "workEmail": "workEmail",
      "personalEmail": "personalEmail",
      "educationType": "educationType",
      "educationTitle": "educationTitle",
      "cityId": "cityId",
      "countryId": "countryId",
      "departmentId": "departmentId",
      "departmentName": "departmentName",
      "companyId": "organizationId",
      "employeeNumber": "employeeNumber",
      "birthday": "birthday",
      "foodPreferences": "foodPreferences",
      "aboutMyJob": "aboutMyJob",
      "aboutMe": "aboutMe",
      "externalProfileId": "externalProfileId",
      "jobTitle": "jobTitle",
      "office": "office"
    },
    "comment": {
      "commentId": "commentId",
      "postId": "postId",
      "activityId": "activityId",
      "body": "body",
      "header": "headline",
      "userName": "userName",
      "userPhoto": "userPhotoUrl",
      "createdAt": "createdAt",
      "likesCount": "likesCount",
      "liked": "liked"
    }
  },
  "assets": { "activityAssetIdentifierStrategy": "postIdOrActivityId" },
  "textFormatting": { "outgoingLineBreak": "crlf" },
  "connection": {
    "apiBaseURL": "https://service.example.org/api",
    "assetsBaseUrl": "https://service.example.org/content",
    "avatarOrgBaseURL": "https://service.example.org/organisation/",
    "webPortalURL": "https://service.example.org",
    "profilerURL": "https://service.example.org/profiler/",
    "tenantID": "00000000-0000-0000-0000-000000000000",
    "clientID": "00000000-0000-0000-0000-000000000000",
    "redirectURI": "msal00000000-0000-0000-0000-000000000000://auth",
    "scopes": ["openid", "profile", "offline_access", "api://00000000-0000-0000-0000-000000000000/access"],
    "authProviderDisplayName": "Microsoft"
  }
}
```
