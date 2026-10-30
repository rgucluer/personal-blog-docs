Changes

```javascript
import { RequiredDataFromCollectionSlug } from 'payload'
import type { PostArgs } from './post-1'


export const post2: (args: PostArgs) => RequiredDataFromCollectionSlug<'posts'> = ({
  heroImage,
  blockImage,
  writer,
}) => {
  return {
    slug: 'global-gaze',
    _status: 'published',
    writers: [writer],
    .....
  }
}

```
