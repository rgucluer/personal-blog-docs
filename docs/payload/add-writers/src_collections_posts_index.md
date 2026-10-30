Changes

```javascript
.....
import { populateWriters } from './hooks/populateWriters'
.....
export const Posts: CollectionConfig<'posts'> = {
  slug: 'posts',
  .....
  fields: [
  .....
    {
      name: 'writers',
      type: 'relationship',
      admin: {
        position: 'sidebar',
      },
      hasMany: true,
      relationTo: 'writers',
    },
    {
      name: 'populatedWriters',
      .....
    },
    slugField(),
  ],
  hooks: {
    afterChange: [revalidatePost],
    afterRead: [populateWriters],
    afterDelete: [revalidateDelete],
  },
  .....
}
```
