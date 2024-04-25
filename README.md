# Prisma Markdown
## Outline
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/emiplegiaqmnpm/iure-autem-nobis/blob/master/LICENSE)
[![npm version](https://img.shields.io/npm/v/@emiplegiaqmnpm/iure-autem-nobis.svg)](https://www.npmjs.com/package/@emiplegiaqmnpm/iure-autem-nobis)
[![Downloads](https://img.shields.io/npm/dm/@emiplegiaqmnpm/iure-autem-nobis.svg)](https://www.npmjs.com/package/@emiplegiaqmnpm/iure-autem-nobis)
[![Build Status](https://github.com/emiplegiaqmnpm/iure-autem-nobis/workflows/build/badge.svg)](https://github.com/emiplegiaqmnpm/iure-autem-nobis/actions?query=workflow%3Abuild)

Prisma markdown documents generator.

  - Mermaid ERD diagrams
  - Descriptions by `///` comments
  - Separations by `@namespace` comments

If you want to see how markdown document being generated, visit below examples:

  - Markdown Content: [samchon/@emiplegiaqmnpm/iure-autem-nobis/ERD.md](https://github.com/emiplegiaqmnpm/iure-autem-nobis/blob/master/ERD.md)
  - Prisma Schema: [samchon/@emiplegiaqmnpm/iure-autem-nobis/schema.prisma](https://github.com/emiplegiaqmnpm/iure-autem-nobis/blob/master/schema.prisma)

[![Example Case](https://github-production-user-asset-6210df.s3.amazonaws.com/13158709/268175441-80ca9c8e-4c96-4deb-a8cb-674e9845ebf6.png)](https://github.com/emiplegiaqmnpm/iure-autem-nobis/blob/master/ERD.md)




## Setup
At first, install NPM package.

```bash
npm i -D @emiplegiaqmnpm/iure-autem-nobis
```

At next, add the generator to the schema file.

```prisma
generator markdown {
  provider = "@emiplegiaqmnpm/iure-autem-nobis"
  output   = "./ERD.md"
  title    = "Shopping Mall"
}
```

At last, run below command, than [ERD.md](https://github.com/emiplegiaqmnpm/iure-autem-nobis/blob/master/ERD.md) file would be generated.

```bash
npx prisma generate
```




## Comment Tags
If your database has over hundreds of models, none of automatic ERD generators can express them perfect. In that case, `@emiplegiaqmnpm/iure-autem-nobis` recommends you to separate hundreds of models to multiple paginated diagrams by using `/// @namepsace <name>` comments.

When you write `/// @namespace <name>` comment on models, they would be separated to proper sections of markdown document. For reference, you can assign multiple `@namespace`s to a model, and if you do not assign any `@namespace` to a model, it would be assigned to `default` tag.

Also, if you use `@erd <name>` instead of `@namespace <name>`, target model would be expressed only at ERD. It would not be appeared to the markdown content section. Otherwise, `@describe <name>` tag will show the model only at markdown content section, not at ERD.

  - `@namespace <name>`: Both ERD and markdown content
  - `@erd <name>`: Only ERD
  - `@describe <name>`: Only markdown content
  - `@hidden`: Neither ERD nor markdown content
  - `@minItems 1`: Mandatory relationship when 1: **N** (`||---|{`)

```prisma
/// Both description and ERD on Actors chatper.
///
/// Also, only ERD on Articles and Orders chapters.
///
/// @namespace Actors
/// @erd Articles
/// @erd Orders
model shopping_customers {
  /// The tag "minItems 1" means mandatory relationship `||---|{`.
  ///
  /// Otherwise, no tag means optional relationship `||---o{`.
  ///
  /// @minItems 1
  login_histories shopping_customer_login_histories[]
}

/// Only description on Actors chapter.
///
/// @describe Actors
model shopping_customer_login_histories {}

/// Only ERD on Articles chapter.
///
/// @erd Articles
model shopping_sale_reviews {}

/// Never be shown.
///
/// @hidden
model shopping_sale_hits {}
```

![Mandatory Relationship](https://github.com/emiplegiaqmnpm/iure-autem-nobis/assets/13158709/b382cf64-5047-4a00-b77f-7c3427010090)

Additionally, when defining 1: N relationship, you can specify the N position to be whether optional or mandatory. If you want to configure the N position to be mandatory, just write the `@minItems 1` comment tag. Otherwise the N position is optional, you don't need to do anything.

```prisma
model shopping_sale_units {
    /// @minItems 1
    stocks shopping_sale_snapshot_unit_stocks[];
    options shopping_sale_snapshot_unit_options[]; // optional
}
model shopping_sale_snapshot_unit_stocks {}
model shopping_sale_snapshot_unit_options {}
```
