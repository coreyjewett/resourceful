var path = require('path'),
    assert = require('assert'),
    events = require('events'),
    vows = require('vows'),
    resourceful = require('../../lib/resourceful');

resourceful.env = 'test';

vows.describe('resourceful/engines/memory').addVows({
  "Creating some resources": {
    topic: function () {
      var MemUser = resourceful.define('MemUser');
      var promise = new(events.EventEmitter);
      var defaults = [
        { _id: 'bob', age: 35, hair: 'black', resource: 'User'},
        { _id: 'tim', age: 16, hair: 'brown', resource: 'User'},
        { _id: 'mat', age: 29, hair: 'black', resource: 'User'}
      ];

      // there are prettier ways to do this.
      MemUser.create(defaults[0], function (e, res) {
        assert.isNull(e);
        MemUser.create(defaults[1], function (e, res) {
          assert.isNull(e);
          MemUser.create(defaults[2], function (e, res) {
            promise.emit('success', res);
          })
        })
      })
      return promise;
    },
    "works": function (e, obj) {
      assert.isNull(e);
      assert.ok(obj);
      var MemUser = resourceful.resources['MemUser'];
      assert(Object.keys(MemUser.connection.store).length, 3);
    }
  }
}).addVows({
  "A Resource" : {
    topic: function() {
      return resourceful.resources['MemUser'];
    },
    "an all() request": {
      topic: function (r) {
        r.all(this.callback);
      },
      "should respond with an array of all records": function (e, obj) {
        assert.isArray(obj);
        assert.equal(obj.length, 3);
      }
    }
  }
}).addVows({
  "A Resource" : {
    topic: function() {
      return resourceful.resources['MemUser'];
    },
    "having": {
      topic: function (r) {
        r.all(this.callback);
      },
      "resources": function (e, obj) {
        assert.isArray(obj);
        assert.equal(obj.length, 3);
      },
      "when truncated": {
        topic: function (r) {
          var MemUser = resourceful.resources['MemUser']
          MemUser.truncate();
          return MemUser;
        },
        "an all() request": {
          topic: function (r) {
            r.all(this.callback);
          },
          "should respond with no records": function (e, obj) {
            assert.isArray(obj);
            assert.equal(obj.length, 0);
          }
        }
      }
    }
  }
}).export(module);