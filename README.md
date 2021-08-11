# Supabase utilities for consuming data

Monorepo that includes several packages related to interactions with supabase.

## Supabase SWR

Allows using [SWR](https://swr.vercel.app/) with simple supabase queries.

## Examples:

```ts
export const { useGetEntries } = getSupabaseSWR(
  process.env.NEXT_PUBLIC_SUPABASE_URL || '',
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY || '',
)

const {
  data: users,
  error,
  isLoading,
} = useGetEntries<Users>(
  session.access_token,
  // Name of the table.
  'users',
  // Fields to get the query from.
  ['name', 'uuid'],
  // Collection of filters
  [
    { field: 'order', relationship: 'name', value: 'asc' },
    { field: 'age', relationship: 'lt', value: '18' },
  ],
  // Range (selects the first 3 items)
  [0, 2],
)
```

You can pretty much use the filters available here that supabase allows for: [https://postgrest.org/en/v7.0.0/api.html](https://postgrest.org/en/v7.0.0/api.html)

## Supabase fetcher helpers

Includes a fetcher function and helpers.

```ts
const apiKey = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY // Or however you store you're key.
const fetcher = getFetcher(apiKey)

const token = session.access_token // User's access token.
const apiUrl = process.env.NEXT_PUBLIC_SUPABASE_URL // https://something.supabase.co
const table = 'consumers'
const { select, filterString, rangeString } = getOptions(fields, filters, range)

const data = await fetcher(
  token,
  apiUrl,
  table,
  select,
  filterString,
  rangeString,
)
```
