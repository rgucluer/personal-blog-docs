Changes

```javascript
import type { Media, Writer } from '@/payload-types'
import { RequiredDataFromCollectionSlug } from 'payload'

export type PostArgs = {
  heroImage: Media
  blockImage: Media
  writer: Writer
}

export const post1: (args: PostArgs) => RequiredDataFromCollectionSlug<'posts'> = ({
  heroImage,
  blockImage,
  writer,
}) => {
  return {
    slug: 'digital-horizons',
    _status: 'published',
    writers: [writer],
    content: {
      root: {
        type: 'root',
        children: [
          {
            type: 'heading',
            .....
          },
          {
            type: 'block',
            fields: {
              blockName: 'Disclaimer',
              blockType: 'banner',
              content: {
                root: {
                  type: 'root',
                  children: [
                    {
                      .....
                      text: 'Disclaimer:',
                      .....
                    },
                    {
                      .....
                      text: 'This content is fabricated .....',
                      .....
                    },
                    {
                      type: 'link',
                      children: [
                        {
                          .....
                          text: 'navigate to the admin dashboard.',
                          .....
                        },
                      ],
                      .....
                    },
                  ],
                  direction: 'ltr',
                  format: '',
                  indent: 0,
                  textFormat: 1,
                  version: 1,
                }
              }
            }
          } 
        ]
      }
    .....
    }
  }
}


```