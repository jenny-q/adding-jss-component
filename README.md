# Adding a component 

> **Note:** There is a command to create these files: `jss scaffold ComponentName`, but they are js files. This is for a react typescript project.

In this example, I will be creating a basic banner component with a header and link.

Start by creating a definition file under `sitecore/definitions/components/Banner.sitecore.ts`. 

Most files will look like this, you should update the fields with the appropriate type based on requirements. Review the `CommonFieldTypes` definition on your local to view all the field types.

```typescript
import { CommonFieldTypes, SitecoreIcon, Manifest } from '@sitecore-jss/sitecore-jss-manifest';

/**
 * Adds the Banner component to the disconnected manifest.
 * This function is invoked by convention (*.sitecore.js) when `jss manifest` is run.
 * @param {Manifest} manifest Manifest instance to add components to
 */
export default function Banner(manifest: Manifest): void {
  manifest.addComponent({
    name: 'Banner',
    displayName: 'Banner',
    icon: SitecoreIcon.DocumentTag,
    fields: [
      { 
        name: 'heading',
        displayName: 'Banner Heading',
        type: CommonFieldTypes.SingleLineText
      },
      { 
        name: 'CTA',
        displayName: 'Banner Link',
        type: CommonFieldTypes.GeneralLink
      }
  ],
  });
}
```

The **name** is the property we'll use to render the item.

**displayName** is what the user will see in sitecore.

**type** is the type of field, e.g image, text, rich text, multilist, etc.


Create the `Banner.tsx` file within `src/components/Banner.tsx`.
The component name should match the sitecore definition name.

When working with typescript:
- don't forget to import the field types (Text, Field, etc)
- define the proper type (line 61 - 66)
- null check 


```typescript
import React from 'react';
import { Text, Field, withDatasourceCheck, LinkField, Link } from '@sitecore-jss/sitecore-jss-nextjs';
import { StyleguideComponentProps } from 'lib/component-props';


type ComponentProps = StyleguideComponentProps & {
  fields: {
    heading: Field<string>;
    CTA: LinkField; 
  };
};

const Banner = ({ fields }: ComponentProps): JSX.Element => {
  return (
    <section className="banner">
        {fields.heading ? (
          <Text tag="h1" className="banner__header" field={fields.heading} />
        ) : (null)}
        {fields.CTA.value.text ? (
          <Link className="button button--yellow" field={fields.CTA.value} />
        ) : null}
    </section>
  )
};

export default withDatasourceCheck()<ComponentProps>(Banner);
```

From here, you can deploy and add the component within your sitecore content editor:
insert image

You can also create a yml file and deploy. When creating a yml file, make sure you are using the proper spacing or the deploy will fail.
Add your file under `data/routes/pageName/en.yml`. Note that if your `pageName` is test, the url will `localhost/test.
insert image

Once you are done, deploy using:
`jss deploy app --acceptCertificate yourPreDefinedCert -c -d`

