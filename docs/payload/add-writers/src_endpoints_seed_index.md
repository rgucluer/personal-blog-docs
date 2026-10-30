Changes

```javascript
.....
const collections: CollectionSlug[] = [
.....
  'writers',
.....
]
.....
export const seed = async ({
  payload,
  req,
}: {
  payload: Payload
  req: PayloadRequest
}): Promise<void> => {
  .....
  payload.logger.info(`— Seeding demo writer ...`)

  await payload.delete({
    collection: 'writers',
    depth: 0,
    where: {
      slug: {
        equals: 'demo-author',
      },
    },
  })
.....
  const [demoWriter, image1Doc, image2Doc, image3Doc, imageHomeDoc] = await Promise.all([
    payload.create({
      collection: 'writers',
      data: {
        title: 'Demo Writer',
        slug: 'demo-writer',
      },
      draft: true,
    }),
    .....
  ])
.....
  const post1Doc = await payload.create({
    .....
    data: post1({ heroImage: image1Doc, blockImage: image2Doc, writer: demoWriter }),
  })

  const post2Doc = await payload.create({
    .....
    data: post2({ heroImage: image2Doc, blockImage: image3Doc, writer: demoWriter }),
  })

  const post3Doc = await payload.create({
    .....
    data: post3({ heroImage: image3Doc, blockImage: image1Doc, writer: demoWriter }),
  })
  .....
  payload.logger.info(`— Seeding globals...`)
.....
  await Promise.all([
    .....
    payload.updateGlobal({
      slug: 'footer',
      data: {
        navItems: [
          {
            link: {
              type: 'custom',
              label: 'Writers',
              url: '/writers',
            },
          },
          .....
        ],
      },
    }),
  ])
  payload.logger.info('Seeded database successfully!')
}

```