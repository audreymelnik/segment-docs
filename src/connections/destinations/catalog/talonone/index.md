---
rewrite: true
title: Talon.One Destination
beta: true
---
Create flexible and targeted promotional & loyalty campaigns with [Talon.One](https://Talon.One/?utm_source=segmentio&utm_medium=docs&utm_campaign=partners).
Campaigns can be created and managed by non-technical users such as marketeers. There is no need to get your development team involved. Features include coupons, discounts, loyalty programs, referral tracking, geo-fencing, and bundling.

This destination is maintained by Talon.One. For any issues with the destination, please [contact their team](mailto:support@talon.one).

_**NOTE:** The Talon.One Destination is currently in beta, which means that they are still actively developing the destination. This doc was last updated on January 15, 2020. If you are interested in joining their beta program or have any feedback to help improve the Talon.One Destination and its documentation, please [let their team know](mailto:support@talon.one)!_

> warning ""
> Data collection that affects promotions should be collected using a Segment **server-side** implementation. Client-side implementations exposes you to risks of fraud. (e.g. a user changing a custom trait relating to their profile via JS modification tools, which triggers them to receive a higher discount than they are entitled to) For more information please [read this](https://segment.com/docs/guides/how-to-guides/collect-on-client-or-server/).

## Getting Started

{% include content/connection-modes.md %}

1. From your Segment UI's Destinations page click on "Add Destination".
2. Search for `Talon.One` within the Destinations Catalog and confirm the Source you'd like to connect to.
3. Drop in the "API Key" into your Segment Settings UI which you can find from your Talon.One Dashboard. Select your Application > Settings > Developer Settings > Create A New API Key
4. Add your Talon.One Application Domain Name e.g. https://demo.talon.one.
5. Add at least one Custom Attribute. These are the Segment `Custom Traits` that link to Talon.One's Customer Profile `Custom Attributes`. More Information below.

## Identify

If you haven't had a chance to review our spec, please take a look to understand what the [Identify method](https://segment.com/docs/spec/identify/) does. An example call would look like:

```js
analytics.identify('userId123', {
  email: 'john.doe@segment.com',
  name: 'John Doe',
});
```

Identify calls are sent to Talon.One as an identify event. The `userId` has a 1-1 mapping to Talon.One's `integrationId`. The `traits` in Segment are mapped with Talon.One's Customer's `custom attributes`.

> note "Note:"
> This app only supports logged in users.

## Custom Attributes

`Custom Attributes` must be set up in the Talon.One Campaign Manager before you make any Identify calls. To set up attributes:

1. From the Campaign Manager, click **Home** > See **Developer Section** > **Create Attributes**
2. Select "Customer Profile" as Associated Entity
3. Select the corresponding Attribute Type (e.g. String).
   For Audiences created in Segment, the corresponding data type is `Boolean` in Talon.One.
4. Enter the API Name (Internal).
   The name is case sensitive, and must be exactly the same as the traits you intend to send from Segment.
5. Add the same name to the Talon.One Destination. On the Segment Dashboard, click **Destinations** > **Talon_One** > **Custom Attributes** > **Add Row** > **Your Custom Attributes** > **Save**.

Parent/child objects are supported by flattening the data using an underscore.
For example:

```js
address:{
   city: 'Berlin'
 }
```
becomes `address_city`.

##  View transferred data

1. Log into your Talon.One Campaign Manager.
2. Select your Application
3. Select the `Customers tab`
4. Enter in the `INTEGRATION ID` (this is case sensitive).
5. The customer data is visible under the `Attributes` tab

## Audience & Computed Traits

`Computed traits` and `audiences` data can be communicated to the Talon.One destination as a customer's `custom attribute`. To learn more about Personas, contact us for a demo.

An **identify** call is sent to the destination for each user being added and removed from an Audience. The trait name is the snake_cased version of the audience name you provide, with a  boolean (`true`/`false`) value.

For example, when a user first completes an order which falls in a time window of the last 30 days, an identify call is sent to Talon.One with the trait `order_completed_last_30days: true`. When this user no longer satisfies this condition, the value is updated to `false` and automatically transmitted to Talon.One.

> note "Note:"
> Similar to `traits/custom traits`, `audiences` and `computed traits` need to be added as `custom attributes` on the Talon.One Campaign Manager. Although unlike `traits/custom traits`, they do not have to be added to the `custom attributes` of this destination application.

When the audience is first created, an identify call is sent for every user in the audience. Subsequent syncs only send updates for those users which were added or removed since the last sync.