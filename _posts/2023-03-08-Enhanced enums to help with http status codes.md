---
title: Enhanced enums to help with http status codes
date: 2023-03-08 11:23:14 +0100
categories: [flutter]
tags: [flutter,dart,enum,http,response]
image:
  path: /assets/img/flutter.png
  alt: extended enums
author: michal_cwiklinski
toc: false
---

During my pet project written in Flutter, I had a need to handle server response codes in order to properly support network requests. First thing I did - I went to [pub.dev](https://pub.dev) and found [http_status_code](https://pub.dev/packages/http_status_code) package.

But then I thought, that I might do it simpler. Since **Dart 2.17**, possibility to use extended enums was introduced.  We can declare enums with members. It also is giving us possibity to:
- define multiple properties for entry
- add named or positional arguments to the constructor (use `const` is a must)
- define custom methods and getters

So - solution was really simple:
```dart
enum StatusCode {
  ok(200, 'OK'),
  created(201, 'OK'),
  accepted(202, 'OK'),
  badRequest(400, 'Bad request'),
  unauthorized(401, 'Unauthorized'),
  forbidden(403, 'Forbidden'),
  notFound(404, 'Not found'),
  internalServerError(500, 'Internal server error')
  serviceUnavailable(503, 'Service Unavailable');

  const StatusCode(this.code, this.description);
  final int code;
  final String description;

  @override
  String toString() => 'StatusCode($code, $description)';

  static StatusCode? fromCode(int code) => StatusCode.values.firstOrNull((value) => value.code = code);
}
```