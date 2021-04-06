# Hints on naming of Belts

### Naming convention recommendations

It has proven that naming Belts with this pattern yields a good readability and overview:

`belt name = belt--<appID>--<usecaseID>`, where

1. `appID` is the name of the application owning the belt
2. `usecaseID` is the name of the concrete data product within the application that is being served by this belt

### Technical limitations

Generally speaking, the Belt API features a `name` property which has a maxium lenght of 255 characters. It is used for displaying purposes only. The technical identifier is the property `id`. Value range for `id` is a positive `Long` value \(i.e. `1` to `9223372036854775807`\).

