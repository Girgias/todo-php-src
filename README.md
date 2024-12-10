# Gina's php-src TODO list

This is a personal TODO lisit of things I noticed in PHP or in php-src's codebase of things I want to fix.
Anyone is free to start tackling these issues as some might be good entry point into contributing to php-src.

Some ideas might require RFC which I may or may not have already marked down as ideas on my other
[RFC ideas](https://github.com/Girgias/php-rfcs) repo.

## Useful macros and links

- <https://heap.space>
- `ZEND_STRL()`

## Behavioural changes

- [ ] Throw ValueError for `log()` with base = 1 instead of returning NAN
- [ ] Fix zval_compare() semantics so that `array_diff()` and `array_intersect()` are better bheaved
- [ ] Do bound checking on `time_t` and `timeval` struct members, see [GH-16246](https://github.com/php/php-src/pull/16246) for an example
- [ ] ValueError for `scandir()` on invalid sort orders
- [ ] ValueError for I/O functions when an empty path is provided
  - [ ] ext/standard
  - [ ] ext/phar
- [ ] Add warning when destructing a scalar value (e.g. (`[$x] = true`) see: [code snippet 1](https://3v4l.org/7lNhK), [code snippet 2](https://3v4l.org/p5LDd), [doc PR](https://github.com/php/doc-en/pull/3680) )
- [ ] `unserialize()` changes:
  - [ ] Add new option to throw when encountering an undefined class instead of creating a `PHP_Incomplete` object
   - [ ] Use a callback or just blanket?
  - [ ] Deprecate not passing the `allowed_classes` option
  - [ ] Option for enabling or not the autoloader
  - [ ] Deprecate INI setting after option has been introduced
- [ ] `__PHP_Incomplete_Class`
  - [ ] Prevent instantiation in userland
  - [ ] Disable cloning
- [ ] Promote warnings to `Error` in `PHP_Incomplete` object handlers
- [ ] Remove `disable_classes` INI setting (see: [PR](https://github.com/php/php-src/pull/12043))
- [ ] Warn when attempting to cast non finite float values to something else (i.e. `NAN`, `INF`, `-INF`)
- [ ] Reflection:
  - [ ] Split ReflectionClass into RelfectionClass, ReflectionInterface, and ReflectionTrait (bunch of methods do not make sense for each individual type)

## Code quality

- [ ] Creatre a C enum for `HASH_KEY_IS_*` constants
- [ ] Better name for `is_vertical` flag in `php_into_print_box_start()`
- [ ] Remove call to `strlen()` in `zend_get_module_version()` ?
- [ ] Enable `-Wshadow` warning (probably will need to only do local as `execute_date` is shadowed a lot (see related [PR](https://github.com/php/php-src/pull/16461))
- [ ] Enable `-Wconversion` warning (note: **huge** effort)
- [ ] ext/hash uses `SUCCESS`/`FAILURE` in unserialize handler for dubious reasons (i.e. it does not return `zend_return`)
- [ ] New Zend API for `bin2hex()` to remove ext/hash usage in:
  - [ ] `ext/standard/string.c`
  - [ ] `ext/hash/php_hash.h`
  - [ ] `Zend/zend_system_id.c`
  - [ ] `ext/opcache/ZendAccelerator/`
  - [ ] `ext/soap/`
    - [ ] `./php_encoding.c`
    - [ ] `./php_http.c` 
    - [ ] Check `to_xml_hexbin()`
  - [ ] Check related  ext/session `bin_to_readable()` function
- [ ] Review usage of forced casts (see [php-tasks issue](https://github.com/php/php-tasks/issues/32))
- [ ] Review validation of INI settings (see [php-tasks issue](https://github.com/php/php-tasks/issues/31))

## Investigations/review

- [ ] `array_combine()` non string behaviour
- [ ] `array_walk(_recursive)()` with objects as HashTable
- [ ] Stream wrappers and stream filters able to disable?
- [ ] `main/network.c`
- [ ] `ext/standard/head.c`
- [ ] `ext/phar` what happens with empty aliases or nul bytes?
  - [ ] Use `P` ZPP specifier for `Phar::addFile()` and `Phar::setAlias()`
- [ ] Check https://wiki.php.net/rfc/inconsistent-behaviors#array_of_chars is still relevant in PHP 8.
- [ ] `php_mblen()` use re-entrant version always or C23 requirement makes this irrelevant

## Deprecation

- [ ] Deprecate `default_dir` behaviour for `readdir()`, `rewinddir()`, and `closedir()`
- [ ] `strip_tags()` and corresponding stream filter (todo check if both were done in 8.1)
- [ ] `str_rot13()` and corresponding stream filter
- [ ] `soundex()`
- [ ] `metaphone()`
- [ ] Deprecate `double` meaning
- [ ] Deprecate non standard casts (See: [RFC](https://wiki.php.net/rfc/deprecate-inconsistent-cast-keywords) from someone else)
- [ ] Deprecate `exclude_disabled` parameter of `get_defined_functions()` as it is useless since 8.0
  - [ ] Amend documentation to reflect this
- [ ] Deprecate `realpath()` with empty string in favour of `getcwd()`
  - [ ] Documentation: Update See Also section to include link to `getcwd()`
