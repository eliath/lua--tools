CSV 2 JSON
==========

Usage
-----

Run the script with torch7.

### Mapping

The script offers a `--map` option that allows the user to apply a custom mapping
function to the CSV rows. The `--map` option expects a path to a lua file that reutrns
a single function. The function has two parameters: the CSV row, as a lua table, and
the row index. The function should return a lua table representing the output for that CSV row

For example, calling the script with mapping function:

    $ th csv2json input.csv output/ --map ~/code/map.lua

`~/code/map.lua`:

    return function(data, index)
      local out = tablex.union(data, {
        fullname = data.firstname .. ' ' .. data.lastname,
        id = index
      })
      return out
    end

The above would take a CSV row, retain all its data as-is, and insert (or overwrite)
the `fullname` and `id` fields.

- `data` is the CSV row as a lua table
- By default, script expects column headers in the first row
- Row `0` for CSVs with column headers will be the first row of actual data, not the header row

If the file has a header row, `data` will have the headers as string keys and the
values as the CSV column values as strings.

If the file does not have a header row, `data` will be a "list" of string values.