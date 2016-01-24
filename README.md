Mongoose Delete Plugin
=========

mongoose-delete is simple and lightweight plugin that enables soft deletion of documents in MongoDB. This code is based on [riyadhalnur's](https://github.com/riyadhalnur) plugin [mongoose-softdelete](https://github.com/riyadhalnur/mongoose-softdelete).

[![Build Status](https://travis-ci.org/dsanel/mongoose-delete.svg?branch=master)](https://travis-ci.org/dsanel/mongoose-delete)

[![Coverage Status](https://coveralls.io/repos/github/dsanel/mongoose-delete/badge.svg?branch=master)](https://coveralls.io/github/dsanel/mongoose-delete?branch=master)

##Features

  - Add __deleted__ (true-false) key on document
  - Add __deletedAt__ key to store time of deletion
  - Add __deletedBy__ key to record who deleted document
  - Restore deleted documents

## Installation
Install using [npm](https://npmjs.org)
```
npm install mongoose-delete
```
## Usage

We can use this plugin with or without options.

### Simple usage

```javascript
var mongoose_delete = require('mongoose-delete');

var PetSchema = new Schema({
    name: String
});

PetSchema.plugin(mongoose_delete);

var Pet = mongoose.model('Pet', PetSchema);

var fluffy = new Pet({ name: 'Fluffy' });

fluffy.save(function () {
    // mongodb: { deleted: false, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6 }

    fluffy.delete(function () {
        // mongodb: { deleted: true, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6 }

        fluffy.restore(function () {
            // mongodb: { deleted: false, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6 }
        });
    });

});

```


### Save time of deletion

```javascript
var mongoose_delete = require('mongoose-delete');

var PetSchema = new Schema({
    name: String
});

PetSchema.plugin(mongoose_delete, { deletedAt : true });

var Pet = mongoose.model('Pet', PetSchema);

var fluffy = new Pet({ name: 'Fluffy' });

fluffy.save(function () {
    // mongodb: { deleted: false, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6 }

    fluffy.delete(function () {
        // mongodb: { deleted: true, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6, deletedAt: ISODate("2014-08-01T10:34:53.171Z")}

        fluffy.restore(function () {
            // mongodb: { deleted: false, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6 }
        });
    });

});

```


### Who has deleted the data?

```javascript
var mongoose_delete = require('mongoose-delete');

var PetSchema = new Schema({
    name: String
});

PetSchema.plugin(mongoose_delete, { deletedBy : true });

var Pet = mongoose.model('Pet', PetSchema);

var fluffy = new Pet({ name: 'Fluffy' });

fluffy.save(function () {
    // mongodb: { deleted: false, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6 }

    var idUser = mongoose.Types.ObjectId("53da93b16b4a6670076b16bf");

    fluffy.delete(idUser, function () {
        // mongodb: { deleted: true, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6, deletedBy: ObjectId("53da93b16b4a6670076b16bf")}

        fluffy.restore(function () {
            // mongodb: { deleted: false, name: 'Fluffy', _id: 53db63bb322236e666c3d7a6 }
        });
    });

});

```

## License

The MIT License

Copyright (c) 2014 Sanel Deljkic http://dsanel.github.io/

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
