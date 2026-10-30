Changes

```javascript
.....
import { formatWriters } from '@/utilities/formatWriters'

export const PostHero: React.FC<{
  post: Post
}> = ({ post }) => {
  const { categories, heroImage, populatedWriters, publishedAt, title } = post

  const hasWriters =
    populatedWriters && populatedWriters.length > 0 && formatWriters(populatedWriters) !== ''

  return (
    <div className="relative .....">
      <div className="container .....">
        <div className="col-start-1 .....">
        .....
          <div className="flex flex-col md:flex-row gap-4 md:gap-16">
            {hasWriters && (
              <div className="flex flex-col gap-4">
                <div className="flex flex-col gap-1">
                  <p className="text-sm">Writer</p>
                  <p>{formatWriters(populatedWriters)}</p>
                </div>
              </div>
            )}
            {publishedAt && (
              .....
            )}
          </div>
        </div>
      </div>
      .....
    </div>
  )
}

```

