---
layout: default
title: Hotels
parent: Extensions
nav_order: 2
---

# Markup for Hotels

This page provides background information on the use of schema.org for marking up hotels and other forms of accommodations. For feature requests, please use the [Github repository](https://github.com/schemaorg/schemaorg).

See also: [acknowledgements](#acks)

## Overview

When modeling hotel-related information in Web content with schema.org, there are three core objects:

*   A **[lodging business](../LodgingBusiness)**, e.g. a hotel, hostel, resort, or a camping site): essentially the place and local business that houses the actual units of the establishment (e.g. hotel rooms). The lodging business can encompass multiple buildings but is in most cases a coherent place.
*   An **[accommodation](../Accommodation)**, i.e. the actually relevant units of the establishment (e.g. hotel rooms, suites, apartments, meeting rooms, camping pitches, etc.). These are the actual objects that are offered for rental.
*   An **[offer](../Offer)** to let a hotel room (or other forms of accommodations) for a particular amount of money and for a given type of usage (e.g. occupancy), typically further constrained by advance booking requirements and other terms and conditions.

In schema.org, an offer for a hotel room is using the same patterns as an offer for any other type of product or service, e.g. a TV set or a welding service. However, a hotel room is not a subclass of [Product](../Product) or [Service](../Service). To enable an offer to be described, use is made of the Multi-Typed Entity (MTE) feature of Schema.org.

When defining an instance of an [Accommodation](../Accommodation), that is offered by a [LodgingBusiness](../LodgingBusiness), it is described as being both the appropriate Type for the accomodation _and_ a [Product](../Product). Thus for example a [Hotel](../Hotel) will make an [Offer](../Offer), or offers, for a [HotelRoom](../HotelRoom), that is also identified as a [Product](../Product). Prices and terms and conditions are thus not properties of the hotel or hotel room, but of the offer for a certain room or room category.

The markup for MTE's is very simple:

*   Microdata:

    ```<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">```

*   RDFa:

    ```<div vocab="https://schema.org/" typeof="HotelRoom Product">```

*   JSON-LD:

    ```"@context": "https://schema.org/",
    "@type": ["HotelRoom","Product"],```

In the case of rental accommodations, it is important to attach the [businessFunction](../businessFunction) property with the value [LeaseOut](http://purl.org/goodrelations/v1#LeaseOut) to the [offer](../Offer), unless you want to sell the property. This allows reusing and extending the same types for dwellings for real estate offers in schema.org.

### Basic Model

The diagram shown below illustrates the basic pattern for describing hotel room offers and offers for other forms of accommodations.

All accommodation Types are subclasses of [https://schema.org/Place](../Place). To use them as the subject of an offer, you should use the MTE technique of assigining your instances to be both an Accomodation subclass and a Product. All types of lodging businesses are subclasses of [https://schema.org/LocalBusiness](../LocalBusiness) which itself is a subclass of [https://schema.org/Place](../Place) and [https://schema.org/Organization](../Organization).

The relationship between an accommodation and the local business it belongs to is represented via the [containedInPlace](../containedInPlace) (from hotel room to hotel) or [containsPlace](../containsPlace) (from hotel to hotel room) properties. The relationship between the offer and the room included in the offer can be represented via either the [offers](../offers) property (from the room product to the offer) or via [itemsOffered](../itemOffered) (from the offer to the room).

The relationship between the lodging business and its offers for rooms is represented via the [makesOffer](../makesOffer) property (from the hotel to the offer) or via the [offeredBy](../offeredBy) property (from the offer to the hotel).

When the offer includes more than just the bare room (e.g. meals) or multiple nights we use the [includesObject](../includesObject) property and one or more [TypeAndQuantityNode](../TypeAndQuantityNode) nodes. 
<img alt="Schema.org pattern for describing hotel room offers" width="960" border="0" src="schema_hotels_1.png" title="Schema.org pattern for describing hotel room offers" />

### Types of Lodging Businesses

Currently, there are explicit types for the following kinds of lodging businesses:

*   [BedAndBreakfast](../BedAndBreakfast)
*   [Campground](../Campground)
*   [Hostel](../Hostel)
*   [Hotel](../Hotel)
*   [Motel](../Motel)
*   [Resort](../Resort)

This list might be extended in the future. For other forms, the generic type [LodgingBusiness](../LodgingBusiness) can be used. The property [additionalType](../additionalType) allows using external identifiers for indicating the exact type in such cases.

### Types of Accommodations

For kinds of accommodations, the following types more specific types are available:

*   [Apartment](../Apartment)
*   [CampingPitch](../CampingPitch)
*   [Room](../Room)
    *   [HotelRoom](../HotelRoom)
    *   [MeetingRoom](../MeetingRoom)
*   [House](../House)
*   [Suite](../Suite)

This list might be extended in the future. For other forms, the generic type [Accommodation](../Accommodation) can be used. The property [additionalType](../additionalType) allows using external identifiers for indicating the exact type in such cases.

## Minimal Example

In the most basic form, we can just model the hotel with basic information like photos and star rating and a price range, but without individual room rates and availability information:

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  <div itemprop="address" itemscope itemtype="https://schema.org/PostalAddress">
    <span itemprop="streetAddress">Technikerstrasse 21</span>
    <span itemprop="postalCode">6020</span>
    <span itemprop="addressLocality">Innsbruck</span>
    <span itemprop="addressRegion">Tyrol</span>,
    <span itemprop="addressCountry">Austria</span>
  </div>
  Phone: <span itemprop="telephone">+43 512 8000-0</span>
  <img itemprop="photo" src="../media/hotel_front.png" alt="Front view of the hotel" />
  Star rating: <span itemprop="starRating" itemscope itemtype="https://schema.org/Rating">
    <meta itemprop="ratingValue" content="4">****
  </span>
  Room rates: <span itemprop="priceRange">$100 - $240</span>
</div>
```

See the page for [https://schema.org/Hotel](../Hotel) for a complete list of additional properties for describing the hotel. Popular properties are also explained in more detail below.

**Note:** In the following, we focus on hotel-related examples, but the basic patterns for other types of lodging businesses are the same. Simply use the more specific types (e.g. [Campground](../Campground) or [MeetingRoom](/MeetingRoom)).

## Information about the Hotel

If your Web site provides more detailed information about your hotel, you can use many additional properties for marking them up with schema.org. Below, we highlight a few prominent properties.

### Geo Position

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  <div itemprop="address" itemscope itemtype="https://schema.org/PostalAddress">
    <span itemprop="streetAddress">Technikerstrasse 21</span>
    <span itemprop="postalCode">6020</span>
    <span itemprop="addressLocality">Innsbruck</span>
    <span itemprop="addressRegion">Tyrol</span>,
    <span itemprop="addressCountry">Austria</span>
  </div>
  <div itemprop="geo" itemscope itemtype="https://schema.org/GeoCoordinates">
    <meta itemprop="latitude" content="47.2649990" />
    <meta itemprop="longitude" content="11.3428720" />
  </div>
</div>
```

### Telephone and Fax

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
   Phone: <span itemprop="telephone">+43 512 8000-0</span>
   Fax: <span itemprop="faxNumber">+43 512 8000-99</span>
</div>
```

### Star Rating

Star ratings are popular indicators for the quality of an accommodation. In many countries, they are awarded by dedicated public or private bodies and are based on well-defined criteria that often also specify minimal amenities for a certain star rating.

#### Simple

You can simply mark-up the star rating as a numerical value.

**Example:** 4-star hotel

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Star rating: <span itemprop="starRating" itemscope itemtype="https://schema.org/Rating">
    <meta itemprop="ratingValue" content="4">****
  </span>
  Room rates: <span itemprop="priceRange">$100 - $240</span>
</div>
```

#### Advanced: Type of Star Rating

If available, it makes sense to specify the origin of the star rating, e.g. the name of the awarding institution. In this case, use a [rating](../Rating) and its [author](../author) property for indicating the party making the rating assessment (e.g. HOTREC, DEHOGA, WHR, or Hotelstars).

**Example:** 4-star hotel rated by the DEHOGA

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Star rating: <span itemprop="starRating" itemscope itemtype="https://schema.org/Rating">
    <span itemprop="author" itemscope itemtype="https://schema.org/Organization">
    <span itemprop="name">DEHOGA</span>
    </span>
    <meta itemprop="ratingValue" content="4">****
  </span>
  Room rates: <span itemprop="priceRange">$100 - $240</span>
</div>
```

### Photos and Logo

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <img itemprop="logo" src="../media/logo.png" alt="hotel logo" />
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  <img itemprop="photo" src="../media/hotel_front.png" alt="Front view of the hotel" />
</div>
```

### Map

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  <a itemprop="hasMap" href="https://www.google.com/maps?ie=UTF8&hq&ll=47.1234,11.1234&z=13">
  Click here for a map.</a>
</div>
```

### Opening Hours (Reception)

You can use the [openingHours](../openingHours) or [openingHoursSpecification](../openingHoursSpecification) properties to indicate the opening hours of the reception of the hotel.

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Reception opening hours:
  <meta itemprop="openingHours" content="Mo-Fr 07:00-23:00">Mon-Fri 07.00-23.00
  <meta itemprop="openingHours" content="Sa 08:00-23:00">Saturdays 08.00-23.00
  <meta itemprop="openingHours" content="Sun 08:00-12:00">Sundays 08.00-12.00
</div>
```

### Reviews and Ratings

You can attach individual (customer) reviews and aggregate ratings using [review](../review) / [Review](../Review) and aggregate ratings using [aggregateRating](/aggregateRating) / [AggregateRating](/AggregateRating).

**Note:** Official hotel star ratings are not reviews but should use the [starRating](/starRating) property.

### Number of Rooms

**Example:** A hotel with 60 bedrooms

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  <span itemprop="numberOfRooms">60</span> rooms.
</div>
```

**Example:** A hotel with 45 double rooms and 15 single rooms

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  <span itemprop="numberOfRooms" itemscope itemtype="https://schema.org/QuantitativeValue">
     <span itemprop="value">45</span>
     <span itemprop="unitText">double rooms</span>
  </span>
  <span itemprop="numberOfRooms" itemscope itemtype="https://schema.org/QuantitativeValue">
     <span itemprop="value">15</span>
     <span itemprop="unitText">single rooms</span>
  </span>
</div>
```

### Renovation and Date of Establishment

For the date of establishment, use [foundingDate](../foundingDate). For information about renovation activities, use [additionalProperty](../additionalProperty) and put the year into its [value](../value) property and details into its description.

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Founded: <span itemprop="foundingDate">1985</span>
  Last Renovation: <span itemprop="additionalProperty"
  itemscope itemtype="https://schema.org/PropertyValue">
    <meta itemprop="name" content="renovation">
    <span itemprop="value">2001</span>
    <span itemprop="description">The three-month project involved a complete update
    of all guestrooms, guest bathrooms and public spaces, including a redesign of
    the hotel&apos;s restaurant.</span>
  </span>
</div>
```

### Membership in Organizations

Use [memberOf](../memberOf) to link to noteworthy organizations.

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Member of the <span itemprop="memberOf" itemscope itemtype="https://schema.org/Organization">
    <span itemprop="name">Austrian Tourist Partner Association</span>.
  </span>
</div>
```

### Hotels Chains

Use [parentOrganization](../parentOrganization) to link a chain hotel to the operating company.

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  An <span itemprop="parentOrganization" itemscope itemtype="https://schema.org/Organization">
    <span itemprop="name">ACME Hotel</span>.
  </span>
</div>
```

### Hotel Features and Services

#### Sauna and Gym

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Hotel Features:
  <ul>
  <li itemprop="amenityFeature" itemscope itemtype="https://schema.org/LocationFeatureSpecification">
    <span itemprop="name">Sauna</span>
    <meta itemprop="value" content="True">
  </li>
  <li itemprop="amenityFeature" itemscope itemtype="https://schema.org/LocationFeatureSpecification">
    <span itemprop="name">Gym</span>
    <meta itemprop="value" content="True">
  </li>
  </ul>
</div>
```

#### Sauna with Opening Hours

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Hotel Features:
  <ul>
  <li itemprop="amenityFeature" itemscope itemtype="https://schema.org/LocationFeatureSpecification">
    <span itemprop="name">Sauna</span>
    <meta itemprop="value" content="True">
    <div itemprop="hoursAvailable" itemtype="https://schema.org/OpeningHoursSpecification">
      <link itemprop="dayOfWeek" href="http://purl.org/goodrelations/v1#Thursday" />(Open Thursdays
      <time itemprop="opens" content="16:00:00"> 4:00 PM</time> -
      <time itemprop="closes" content="21:00:00"> 9:00 PM</time>)
    </div>
  </li>
  </ul>
</div>
```

### Check-in and Check-out Times

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <img itemprop="logo" src="../media/logo.png" alt="hotel logo" />
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Check-in: <meta itemprop="checkinTime" content="14:00:00">2:00 PM
  Check-out: <meta itemprop="checkoutTime" content="11:00:00">11:00 AM
</div>
```

### Languages

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  We speak:
  <ul>
    <li itemprop="availableLanguage" itemscope itemtype="https://schema.org/Language">
      <span itemprop="name">English</span>
    </li>
    <li itemprop="availableLanguage" itemscope itemtype="https://schema.org/Language">
      <span itemprop="name">German</span>
    </li>
  </ul>
</div>
```

## Information about Rooms

Some hotels just indicate basic information about the number and type of available room categories. In the simplest form, one can use the [numberOfRooms](../numberOfRooms) property with the type of room in the [unitText](../unitText) property of a [QuantitativeValue](/QuantitativeValue), like so:

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <img itemprop="logo" src="../media/logo.png" alt="hotel logo" />
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  <span itemprop="numberOfRooms" itemscope itemtype="https://schema.org/QuantitativeValue">
     <span itemprop="value">45</span>
     <span itemprop="unitText">double rooms</span>
  </span>
  <span itemprop="numberOfRooms" itemscope itemtype="https://schema.org/QuantitativeValue">
     <span itemprop="value">15</span>
     <span itemprop="unitText">single rooms</span>
  </span>
</div>
```

In many cases, however, hotel Web sites contain additional information like images and textual descriptions about room categories (like "Hollywood Twin Room") or even individual rooms (in particular in the case of small, family-owned hotels). Schema.org provides support for describing rooms and linking them to the hotel. This is also needed if you want to describe actual room offers (see below).

### Basic Pattern: Description and Picture

You can link the hotel to its rooms (or suites or guest appartments) via the [containsPlace](../containsPlace) property or a room to the hotel it belongs to via the inverse [containedInPlace](../containedInPlace).

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Room types:
  <ul>
    <li itemprop="containsPlace" itemscope itemtype="https://schema.org/HotelRoom">
      <span itemprop="name">Double Room</span>
      <span itemprop="description">Our double rooms are spacious and comfortable.</span>
      <img href="../images/double_room.png" />
    </li>
    <li itemprop="containsPlace" itemscope itemtype="https://schema.org/HotelRoom">
      <span itemprop="name">Single Room</span>
      <span itemprop="description">Our single rooms are cosy and comfortable.</span>
      <img href="../images/single_room.png" />
    </li>
  </ul>
</div>
```

### Beds and Occupancy

You can add more details to the [hotel room](../HotelRoom) entities, like the type and number of beds and the occupancy. The occupancy can be a single value (using [value](../value)) or a range (using [minValue](../minValue) and [minValue](../minValue)).

```
<div itemscope itemtype="https://schema.org/Hotel">
  <h1><span itemprop="name">ACME Hotel Innsbruck</span></h1>
  <span itemprop="description">A beautifully located business hotel right in the
heart of the alps. Watch the sun rise over the scenic Inn valley while
enjoying your morning coffee.</span>
  Room types:
  <ul>
    <li itemprop="containsPlace" itemscope itemtype="https://schema.org/HotelRoom">
      <span itemprop="name">Double Room</span>
      <span itemprop="description">Our double rooms are spacious and comfortable.</span>
      Bed type: <span itemprop="bed">Olympic Queen bed</span>
      Occupancy: <span itemprop="occupancy" itemscope itemtype="https://schema.org/QuantitativeValue">
      Up to <span itemprop="maxValue">2</span>
      <meta itemprop="unitCode" content="IE">people
      </span>
    </li>
  </ul>
</div>
```

### Features and Services

Room features, like "mini-bar", "wifi" or "air conditioning" can be expressed using the [amenityFeature](/amenityFeature) property and an [LocationFeatureSpecification](/LocationFeatureSpecification) entity, which is a special form of a [property-value](../PropertyValue). The [amenityFeature](../amenityFeature) property does not make a statement on whether the room feature is free or for an additional charge.

#### Room Features

**Example:** The rooms have a mini-bar and wifi. Charges may or may not apply for those.

```
<div itemscope itemtype="https://schema.org/HotelRoom">
 <span itemprop="name">Double Room</span>
 <span itemprop="description">Our double rooms are spacious and comfortable.</span>
  Room Features:
 <ul>
   <li itemprop="amenityFeature" itemscope itemtype="https://schema.org/LocationFeatureSpecification">
     <span itemprop="name">Mini-bar</span>
     <meta itemprop="value" content="True">
   </li>
   <li itemprop="amenityFeature" itemscope itemtype="https://schema.org/LocationFeatureSpecification">
     <span itemprop="name">Wifi</span>
     <meta itemprop="value" content="True">
   </li>
 </ul>
</div>
```

**Note:** For modeling the fees of optional room features, you can link the [offer](../Offer) for the room to an offer for the feature or service via the [addOn](../addOn) property.

## Offers and Pricing

A key principle in schema.org is that things do not have prices, but that offers to grant you some rights on the things do have instead. Thus, all commercial aspects of a hotel offer belong to the [offer](../Offer) type in schema.org. In the following, we show the use of relevant schema.org elements. Note the use of MTEs, asignining the accomodation instance to also be a [https://schema.org/Product](../Product).

### Basic Pattern

**Example:** Single room for $ 99 per night

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">
  <span itemprop="name">Single Room</span>
  <span itemprop="description">Our single rooms are cosy and comfortable.</span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="99.00">$99.00
      <meta itemprop="priceCurrency" content="USD">
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
</div>
```

### Price per Room vs. Price per Person

You can indicate room prices per room or per person by choosing the appropriate unit code. "IE" is the UN/CEFACT Common Code for persons and "DAY" the code for days / nights. For price information per person you should indicate the occupancy as a single value.

**Example:** Double room for $ 49 per night per person based on double occupancy

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">
  <span itemprop="name">Double Room</span>
  <span itemprop="description">Our double rooms are spacious and comfortable.</span>
  Occupancy: <span itemprop="occupancy" itemscope itemtype="https://schema.org/QuantitativeValue">
      <span itemprop="value">2</span>
      <meta itemprop="unitCode" content="IE">people
  </span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="49.00">$49.00
      <meta itemprop="priceCurrency" content="USD">
      <!-- IE is the UN/CEFACT Code for person -->
      <meta itemprop="unitCode" content="IE">per person (based on double occupancy)
    </span>
  </span>
</div>
```

### Price Ranges

You can use the [minPrice](../minPrice) and [maxPrice](../maxPrice) properties to indicate price ranges for rooms.

**Example:** Single rooms cost between $ 99 and $ 179 per night.

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">
  <span itemprop="name">Single Room</span>
  <span itemprop="description">Our single rooms are cosy and comfortable.</span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="minPrice" content="99.00">$99.00 -
      <meta itemprop="maxPrice" content="179.00">179.00
      <meta itemprop="priceCurrency" content="USD">
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
</div>
```

### Validity of an Offer

You can constrain the validity of a price by time in one of the following ways:

#### Booking Period

**Example:** A rate is valid only when the booking is made between October 1 and December 31, 2015.

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">
  <span itemprop="name">Single Room</span>
  <span itemprop="description">Our single rooms are cosy and comfortable.</span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      Price valid if booked between: <meta itemprop="validFrom" content="2015-10-01">October 1 -
      <meta itemprop="validThrough" content="2015-12-31">December 31, 2015
      <meta itemprop="price" content="99.00">$99.00
      <meta itemprop="priceCurrency" content="USD">
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
</div>
```

#### Advance Booking Requirement

**Example:** A room must be booked seven days in advance.

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">
  <span itemprop="name">Single Room</span>
  <span itemprop="description">Our single rooms are cosy and comfortable.</span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    Must be booked at least <span itemprop="advanceBookingRequirement"
    itemscope itemtype="https://schema.org/QuantitativeValue">
      <span itemprop="minValue">7</span>
      <meta itemprop="unitCode" content="DAY"> days before the stay
    </span>
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="99.00">$99.00
      <meta itemprop="priceCurrency" content="USD">
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
</div>
```

#### Eligible Dates

**Example:** A rate is valid only for stays between January 15-31, 2016.

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">
  <span itemprop="name">Single Room</span>
  <span itemprop="description">Our single rooms are cosy and comfortable.</span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    For stays between: <meta itemprop="availabilityStarts" content="2016-01-15">January 15 -
    <meta itemprop="availabilityEnds" content="2016-01-31">31, 2016
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="99.00">$99.00
      <meta itemprop="priceCurrency" content="USD">
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
</div>
```

#### Seasonal Pricing

**Example:** Different prices for summer and winter

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product">
  <span itemprop="name">Single Room</span>
  <span itemprop="description">Our single rooms are cosy and comfortable.</span>
  Rate:
  <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    Summer <meta itemprop="availabilityStarts" content="2016-06-01"> (June 1 -
    <meta itemprop="availabilityEnds" content="2016-10-31">October 31)
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="69.00">$69.00
      <meta itemprop="priceCurrency" content="USD">
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
  <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    Winter <meta itemprop="availabilityStarts" content="2016-11-01"> (November 1 -
    <meta itemprop="availabilityEnds" content="2017-05-31">May 31)
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="99.00">$99.00
      <meta itemprop="priceCurrency" content="USD">
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
</div>
```

### Compound Prices

For some types of accommodations, the price has multiple components, e.g. a weekly fee and a flat cleaning and service fee for a vacation home. The [CompoundPriceSpecification](../CompoundPriceSpecification) type serves this purpose.

**Example:** 400 USD per week plus a cleaning fee of 100 USD per stay

```
<div itemscope itemtype="https://schema.org/Apartment https://schema.org/Product">
  <span itemprop="name">Seaside Apartment</span>
  <span itemprop="description">Our seaside apartments are perfect for small families with children.</span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/CompoundPriceSpecification">
      <span itemprop="priceComponent" itemscope itemtype="https://schema.org/UnitPriceSpecification">
        <meta itemprop="price" content="400.00">$400.00
        <meta itemprop="priceCurrency" content="USD">
        <!-- WEE is the UN/CEFACT Code for week -->
        <meta itemprop="unitCode" content="WEE"> per week
      </span>plus a
      <span itemprop="priceComponent" itemscope itemtype="https://schema.org/UnitPriceSpecification">
        <span itemprop="name">cleaning fee</span> of
        <meta itemprop="price" content="100.00">$100.00
        <meta itemprop="priceCurrency" content="USD">
        <!-- C62 is the UN/CEFACT Code for no unit -->
        <meta itemprop="unitCode" content="C62"> per stay
      </span>
    </span>
  </span>
</div>
```

### Occupancy

The occupancy for a given room rate can be indicated via [occupancy](../occupancy) (total occupancy). For details (e.g. number of adults vs. number of children), you can use the [name](../name) property of the [value](../QuantitativeValue).

**Example:** USD 249 per night for a suite with up to two adults and two children

```
<div itemscope itemtype="https://schema.org/Suite https://schema.org/Product">
  <span itemprop="name">Deluxe Suite</span>
  <span itemprop="description">Our deluxe suites are ideal for longer stays or families.</span>
  Occupancy: <span itemprop="occupancy" itemscope itemtype="https://schema.org/QuantitativeValue">
      Up to <span itemprop="maxValue">2</span>
      <meta itemprop="unitCode" content="IE">
      <span itemprop="name">adults</span>
  </span> and
  <span itemprop="occupancy" itemscope itemtype="https://schema.org/QuantitativeValue">
    <span itemprop="maxValue">2</span>
    <meta itemprop="unitCode" content="IE">
    <span itemprop="name">children</span>
  </span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="249.00">$249.00
      <meta itemprop="priceCurrency" content="USD">
      <!-- DAY is the UN/CEFACT Code for day/night -->
      <meta itemprop="unitCode" content="DAY">per night
    </span>
  </span>
</div>
```

### Minimum Duration of Stay

**Example:** The room rate is valid only for stays of at least three nights.

```
<div itemscope itemtype="https://schema.org/HotelRoom https://schema.org/Product ">
  <span itemprop="name">Single Room</span>
  <span itemprop="description">Our single rooms are cosy and comfortable.</span>
  Rate: <span itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
    <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
      <meta itemprop="price" content="99.00">$99.00
      <meta itemprop="priceCurrency" content="USD">
      <!-- DAY is the UN/CEFACT Code for day/night -->
      <meta itemprop="unitCode" content="DAY"> per night
    </span> for stays of at least
    <span itemprop="eligibleDuration" itemscope itemtype="https://schema.org/QuantitativeValue">
      <span itemprop="minValue">3</span>
      <meta itemprop="unitCode" content="DAY"> nights
    </span>
  </span>
</div>
```

### Cancellation Rules and Terms and Conditions

Cancellation policy information and other details of the terms and conditions should be put into the [description](../description) property of the [offer](../Offer).

### Fees for Optional Services

Price information for optional services should be put into an additional [offer](../Offer) and linked to from the base offer via the [addOn](../addOn) property.

## Meals and Food Services

Information about meals can be attached to the [offer](../Offer) by creating a product bundle with multiple [TypeAndQuantityNode](../TypeAndQuantityNode) elements and [FoodService](../FoodService).

**Example:** Room with breakfast included

```
<div itemscope itemtype="https://schema.org/Offer">
  <link itemprop="businessFunction" href="http://purl.org/goodrelations/v1#LeaseOut" />
  <span itemprop="name">Single room with breakfast</span>
  <span itemprop="priceSpecification" itemscope itemtype="https://schema.org/UnitPriceSpecification">
    <meta itemprop="price" content="99.00">$99.00
    <meta itemprop="priceCurrency" content="USD">
    <meta itemprop="unitCode" content="DAY">per night
  </span>
  <span itemprop="includesObject" itemscope itemtype="TypeAndQuantityNode">
    <span itemprop="typeOfGood" itemscope itemtype="https://schema.org/HotelRoom">
      <span itemprop="name">Single Room</span>
      <span itemprop="description">Our single rooms are cosy and comfortable.</span>
    </span>
  </span>
  <span itemprop="includesObject" itemscope itemtype="TypeAndQuantityNode">
    <span itemprop="typeOfGood" itemscope itemtype="https://schema.org/FoodService">
      <span itemprop="name">Breakfast</span>
      <span itemprop="description">Our famous healthy starter breakfast is included.</span>
    </span>
  </span>
</div>
```

## Acknowledgments

Many thanks to [Martin Hepp](http://www.heppnetz.de/) for contributing the initial version of this document, and leading the design of schema.org's hotel/accomodation vocabulary.

The elements for describing hotels in schema.org are based on the STI Accommodation Ontology, see [http://ontologies.sti-innsbruck.at/acco/ns.html](http://ontologies.sti-innsbruck.at/acco/ns.html) for details. Many class and property definitions are inspired by or based on abstracts from Wikipedia, the free encyclopedia.