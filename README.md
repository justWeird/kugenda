# kugenda
Kugenda Cross Platform Repo

Overview

Goal: Replace CMU Africa’s WhatsApp-based bus tracking with a unified app that lets drivers broadcast real-time routes and students view shuttles and schedules.

Tech stack: React Native (Expo) + TypeScript (Cloud Functions) + Firebase (Firestore, Realtime DB, Auth, Analytics)

Scope: To deliver an MVP and make room for further development, we have defined a scope for the first deliverable. This cope highlights two major deliverable features:
 - Drivers: Start/stop sharing, view assigned route, and broadcast location.
 - Students: View active shuttles, estimated arrival time, and select preferred routes.

For future development, we defer some other features such as:
Route optimization, notifications by ETA, offline caching, feedback forms.

Architecture Planning: For cross-platform support, we are building with React Native (Expo) because of its ability to work on Android, iOS and web. Other parts of the architecture are shown below:
 - Backend: TypeScript (Typescript.js + Firebase Functions)
 - Database: Firebase Firestore (structured storage) + Firebase Realtime DB (for live tracking)
 - Auth: Google OAuth restricted to @andrew.cmu.edu
 - Analytics: Firebase Analytics + Cloud Logging
 - Hosting: Firebase Hosting (for web), Cloud Functions for APIs



## Architecture Diagram

![Architecture Diagram](./architecture-diagram.svg)

Data models: Key data model objects are defined below. They are stored in firebase firestore as documents. 
 | Entity      | Fields                                                                      | Notes                                    |
| ----------- | --------------------------------------------------------------------------- | ---------------------------------------- |
| **User**    | `uid`, `name`, `role`, `email`, `preferences`, `lastSignIn`, `lastActive`   | Pref: `{ lang, notification }`           |
| **Stop**    | `name`, `lat`, `lng`, `routeRef`                                            | Indexed for distance calc                |
| **Route**   | `name`, `stops[]`, `schedule`, `activeSessions[]`                           | Each session = one live driver broadcast |
| **Bus**     | `name`, `routeRef`, `currentLocation`, `status`                             | Linked to `driverUID`                    |
| **Session** | `sessionId`, `driverUID`, `routeRef`, `locations[]`, `startTime`, `endTime` | Stores route playback data               |

