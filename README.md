# DepPak - Deploy Dependency Packages

A small utility inspired by the "dependencies" section in `build.zig.zon`.

Allows setting dependencies from a file pointing to archive files, including hash-checking.

Archive files are downloaded to `~/.local/var/deppak/cache/`

Reads a JSON file which has the following format:

```js
[
    {
        "hash": "abcd1234",
        "url": "https://some_url/file.tgz",

        // "deploy" is optional and defaults to {"./", ["./"]}
        // that is, copy all contents into local directory
        "deploy" : {
            // copy from archive's `appcode/` location to the deploy location's `src/feature` subdir
            //    and to the "submod/also/uses/feature" folder (as actual duplicate files)
            "appcode/" : ["src/feature", "submod/also/uses/feature"]
        }
    },
    {
        // ... other sources specs ...
    }
]
```

* This will store the archive file at `~/.local/var/deppak/cache/abcd1234/file.tgz`
* DepPak will unpack the archive, and move the contents of `appcode/` into `./src/feature`

This syncs all the specified archives, validates the hash, and unpacks it to a location.

Hash can be specified as `"-"` to cause the downloaded item's hash to be printed to console. Unpacking will not proceed in this case. Re-run with the hash populated to the file to proceed.

See [command examples](command_examples.md) for additional information

## Applications

This can be used where any bunch of distributable files is to be collected.

I would have happily used this for distributing a spec list of Minetest mods for a server, for example.

Any language, project or application that doesn't have its own bundle distribution system can use this.

As a library, this can be integrated into an application for it to provide extensibility with user plugins.

## Goals

* Simple specification of file
* For naive file distribution and collection use-cases
    * unpacks and copies files (as opposed to doing any builds)
    * trying to integrate command dispatch or build scripts could turn it into a pandora-box
* Standalone commandline utility as single binary with no runtime dependencies
* Available as a library to integrate in other applications
* If it becomes relevant, compatibility priority order is Linux, BSD, Windows, Mac
* Weak copyleft. DepPak itself belongs to the community; but can be integrated in proprietary applications.

## License

Lesser GPL, 3.0

This means you can link-against/embed it in a proprietary system without affecting the license of your own project.

If you do distribute a modified version of deppak itself however, or use modified deppak code in an application you distribute, you must release the modified source of your deppak copy to whomever should ask. This is limited to deppak code, and does not apply to any code that calls it or software that embeds it.
