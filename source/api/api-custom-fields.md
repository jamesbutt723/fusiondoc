Custom Fields API
=============

Custom Fields are implemented using React's [PropTypes](https://github.com/facebook/prop-types) library. They are available on both [Feature](./api-feature.md) and [Chain](./api-chain.md) type components.

Fusion will look for a static property on your component named `propTypes`, which should be an object. This object should contain a `customFields` key, whose value is `PropTypes.shape()`. `PropTypes.shape()` expects a single argument, which is an object whose keys are the names of the custom fields you are enumerating, and the values being their `PropTypes`.

Types
-----

### bool

Also aliased as `PropTypes.boolean`.

##### Description

Will produce a checkbox input in PageBuilder and return `true` or `false` depending on the selection.

##### Example

    import PropTypes from 'prop-types';
    import React from 'react'
    
    const MyComponent = (props) => {
      const { showDesc } = props.customFields
    
      return (
        <h1>Hello World!</h1>
        {showDesc && (<p>Lorem ipsum</p>)}
      )
    }
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        myBool: PropTypes.boolean
      })
    }
    
    export default MyComponent
    

### contentConfig()

##### Description

Specifies the content schema that this component is compatible with, so that a PageBuilder user can select from a dynamic list of content sources that match that schema. Once the editor has selected the content source to use, PageBuilder allows the user to set the parameters of that content source so they can be used by the feature to fetch content.

The `contentConfig` custom field returns an object with two keys to the feature: a string representing the content source name is returned to the component as `contentService`, and the parameters to query the content source are returned as an object named `contentConfigValues`.

##### Example

    import PropTypes from 'prop-types'
    import Consumer from 'fusion:consumer'
    import React, { Component } from 'react'
    
    @Consumer
    class Songs extends Component {
      constructor(props) {
        super(props)
    
        const { songListConfig } = this.props.customFields
    
        const { contentService, contentConfigValues } = songListConfig
    
        this.fetchContent({
          songList: {
            source: contentService,
            query: contentConfigValues,
            filter: '{ results { name } }'
          }
        })
      }
    
      render() {
        <ul>
          {this.state.songList.map(song =>
            <li>{song.name}</li>
          )}
        </ul>
      }
    }
    
    Songs.propTypes = {
      customFields: PropTypes.shape({
        songListConfig: PropTypes.contentConfig('songSchema')
      })
    }
    
    export default Songs
    

### date

##### Description

Will produce a datepicker input in PageBuilder and return a string containing the user selected date.

For Fusion's purposes, this is an alias for `PropTypes.string`. It exists only to denote that this field should render a datepicker in PageBuilder.

##### Example

See `PropTypes.string` example.

### dateTime

##### Description

Will produce a date-and-time picker input in PageBuilder and return a string containing the user selected date and time.

For Fusion's purposes, this is an alias for `PropTypes.string`. It exists only to denote that this field should render a date-and-time picker in PageBuilder.

##### Example

See `PropTypes.string` example.

### disabled

##### Description

Will produce a disabled textarea in PageBuilder and return a string containing the value of the textarea. This is commonly used for custom fields that use plugins to set the field value rather than users editing the textarea directly.

For Fusion's purposes, this is an alias for `PropTypes.string`. It exists only to denote that this field should render a disabled textarea in PageBuilder.

##### Example

See `PropTypes.string` example.

### email

##### Description

Will produce an email input in PageBuilder and return a string containing the email the user entered.

For Fusion's purposes, this is an alias for `PropTypes.string`. It exists only to denote that this field should render an email input that validates proper email format in PageBuilder.

##### Example

See `PropTypes.string` example.

### json

##### Description

Will produce a textarea in PageBuilder and return a string containing the value of the textarea, which is expected to be a valid JSON string.

Fusion will attempt to validate that the returned string is in proper JSON format, and will log a warning if this is not the case.

##### Example

    import PropTypes from 'prop-types';
    import React from 'react'
    
    const MyComponent = (props) => {
      const { jsonStr } = props.customFields
    
      return (
        <pre>{jsonStr}</pre>
      )
    }
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        jsonStr: PropTypes.json
      })
    }
    
    export default MyComponent
    

### kvp

##### Description

Will produce a series of "key/value pair" text inputs that allow a PageBuilder editor to set the keys and values of an object, and have that object returned to the component.

##### Example

    import PropTypes from 'prop-types';
    import React from 'react'
    
    const ElectionResults = (props) => {
      const { candidateVotePercent } = props.customFields
    
      return (
        <ul>
          {Object.keys(candidateVotePercent).map(candidateName =>
            <li>
              <strong>{candidateName}</strong> has {candidateVotePercent[candidateName]} of the vote so far
            </li>
          )}
        </ul>
      )
    }
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        candidateVotePercent: PropTypes.kvp
      })
    }
    
    export default MyComponent
    

### label

##### Description

Will produce a simple label in PageBuilder displaying the name of the custom field and a description (if provided). This type of field is used for providing contextual information in the PageBuilder interface from a particular component. It does not return data back to Fusion itself, it is purely for the PageBuilder editorial experience.

##### Example

See `PropTypes.string` example.

### list

##### Description

Will produce a series of text inputs in PageBuilder that allows the editor to enter many values, which will be returned as an array of strings to the component.

##### Example

    import PropTypes from 'prop-types';
    import React from 'react'
    
    const TopicsList = (props) => {
      const { topics } = props.customFields
    
      return (
        <ul>
          {topics.map(topic =>
            <li>{topic}</li>
          )}
        </ul>
      )
    }
    
    TopicsList.propTypes = {
      customFields: PropTypes.shape({
        topics: PropTypes.list
      })
    }
    
    export default TopicsList
    

### number

##### Description

Will produce a `number` input field in PageBuilder that allows users to enter a number value, which then gets returned to the component.

##### Example

    import PropTypes from 'prop-types';
    import React from 'react'
    
    const MyComponent = (props) => {
      const { description } = props.globalContent
      const { maxChars } = props.customFields
    
      const slicedContent = description.slice(0, maxChars)
    
      return (
        <p>{slicedContent}</p>
      )
    }
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        maxChars: PropTypes.number
      })
    }
    
    export default MyComponent
    

### oneOf()

Also aliased as `select()`

##### Description

Will produce a `select` input in PageBuilder that allows users to select from one of the enumerated options in the array. Values in the array can be matched to human-readable text via the `labels` property passed to the [<code>tag</code>](#tag) method.

##### Example

    import PropTypes from 'prop-types';
    import React from 'react'
    
    const MyComponent = (props) => {
      const { listType } = props.customFields
      const El = (listType === 'ol') ? 'ol' : 'ul'
      return (
        <El>
          <li>Foo</li>
          <li>Bar</li>
          <li>Baz</li>
        </El>
      )
    }
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        listType: PropTypes.oneOf([
          'ol', 'ul'
        ]).tag({
          labels: {
            ol: 'Ordered List',
            ul: 'Unordered List'
          }
        })
      })
    }
    
    export default MyComponent
    

### richtext

##### Description

Will produce a textarea in PageBuilder and return a string containing the value of the textarea.

For Fusion's purposes, this is an alias for `PropTypes.string`. It exists only to denote that this field should render a textarea in PageBuilder.

##### Example

See `PropTypes.string` example.

### string

Also aliased as `text`

##### Description

Will produce a text input in PageBuilder that allows users to type a string value, which then gets returned to the component.

##### Example

    import PropTypes from 'prop-types';
    import React from 'react'
    
    const MyComponent = (props) => {
      const { headline } props.customFields
    
      return (<h1>{headline}</h1>)
    }
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        headline: PropTypes.string
      })
    }
    
    export default MyComponent
    

### url

##### Description

Will produce a URL input in PageBuilder and return a string containing the URL the user entered.

For Fusion's purposes, this is an alias for `PropTypes.string`. It exists only to denote that this field should render a URL input that validates proper URL format in PageBuilder.

##### Example

See `PropTypes.string` example.

Global Options
--------------

These options are available as properties/methods that can be added to any Fusion PropType.

### isRequired

##### Description

Denotes that a certain prop is required. Will throw warnings in development if the chosen prop is not present.

##### Example

    ...
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        headline: PropTypes.string.isRequired
      })
    }
    
    export default MyComponent
    

### tag()

##### Description

The `tag()` method provides a way of adding additional metadata that PageBuilder requires about a custom field. It takes an object containing this metadata in key/value pair form as an argument.

##### Parameters

`tag(optionMap)`

*   `optionMap` (_Object_): A map of metadata options about this particular custom field.
    *   `optionMap.defaultValue` (_?_): The default value this custom field should take.
    *   `optionMap.description` (_String_): A text description about the purpose of this custom field for users to better understand it.
    *   `optionMap.group` (_String_): The name of a group of common custom fields. PageBuilder will aggregate custom fields with the same `group` name into a common UI interface element.
    *   `optionMap.formPlugin` (_String_): The name of a plugin used by this custom field. See the Plugin API for more info.
    *   `optionMap.format` (_String_): An [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) compliant date format string, for datepicker custom fields.
    *   `optionMap.hidden` (_Boolean_): Whether to show or hide the custom field.
    *   `optionMap.label` (_Object_ or _String_): An object mapping an i18n locale short code to a translated string. Optionally, a String can be provided to always use the same human-readable label regardless of locale.
    *   `optionMap.labels` (_Object_): An object mapping a value listed in the array of a `oneOf` custom field to a more human-readable string
    *   `optionMap.max` (_Number_): Maximum number allowed for a `number` type custom field.
    *   `optionMap.min` (_Number_): Minimum number allowed for a `number` type custom field.
    *   `optionMap.step` (_Number_): Interval to increase or decrease by for every change to a `number` type custom field.

##### Example

    ...
    
    MyComponent.propTypes = {
      customFields: PropTypes.shape({
        myNumberField: PropTypes.number.tag({
          label: {
            en: 'My Number',
            es: 'Mi Número'
          },
          group: 'examples',
          hidden: false,
          max: 100,
          min: 0,
          step: 5
        }),
        mySelectField: PropTypes.oneOf([
          'foo', 'bar', 'baz'
        ]).tag({
          defaultValue: 'bar',
          description: 'This custom field is useless',
          group: 'examples',
          labels: { foo: 'Foo', bar: 'Bar', baz: 'Baz' }
        })
      })
    }
    
    export default MyComponent