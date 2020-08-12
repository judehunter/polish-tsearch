## Polish .affix, .dict, .stop files for PostgreSQL Text Search

This repository has all the files you need to add the Polish dictionary to your PostgreSQL database:
```
polish.affix
polish.dict
polish.stop
```
All with the correct `UTF-8` encoding (instead of `iso-8859-2`) and names

#### Licensing

This repository uses `The Unlicense` license (public domain).

`.affix` and `.dict` files come from [sjp.pl](https://sjp.pl/slownik/ort/) (`sjp-ispell-pl-%date%-src.tar.bz2`), originally in the `iso-8859-2` encoding and named `.aff`, `.all`.

`.stop` file comes from https://raw.githubusercontent.com/dominem/stopwords/master/polish.stopwords.txt

## Usage

Just move the files to `share/tsearch_data` (`Unix`: where share means the PostgreSQL installation's shared-data directory. `Windows`: the path is relative to PostgreSQL installation directory).

Then, run the following on your database:
```postgres
BEGIN;
      CREATE TEXT SEARCH CONFIGURATION public.polish ( COPY = pg_catalog.english );

      CREATE TEXT SEARCH DICTIONARY polish_ispell (
        TEMPLATE = ispell,
        DictFile = polish,
        AffFile = polish,
        StopWords = polish
      );

      ALTER TEXT SEARCH CONFIGURATION polish
        ALTER MAPPING FOR asciiword, asciihword, hword_asciipart, word, hword, hword_part
        WITH polish_ispell;
COMMIT;
```
