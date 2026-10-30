Changes

```javascript
import { RequiredDataFromCollectionSlug } from 'payload'
import type { PostArgs } from './post-1'


export const post3: (args: PostArgs) => RequiredDataFromCollectionSlug<'posts'> = ({
  heroImage,
  blockImage,
  writer,
}) => {
  return {
    slug: 'dollar-and-sense-the-financial-forecast',
    _status: 'published',
    writers: [writer],
    .....
  }
}

```
