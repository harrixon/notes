# Jasmine

##### For doing test cases

```js
describe("Player", function () {
    var Player = require('../../lib/jasmine_examples/Player');
    var Song = require('../../lib/jasmine_examples/Song');
    
    // put anything that dulpicate into beforeEach
	beforeEach(function () {
    	// use `this` to simplify code
    	this.player = new Player();
    	this.song = new Song();
	});

	it("should be able to play a Song", function () {
    	this.player.play(song);
   		expect(this.player.currentlyPlayingSong).toEqual(this.song);

    	//demonstrates use of custom matcher
    	expect(this.player).toBePlaying(this.song);
	});
});
```

##### Helper functions

```js
// SpecHelper.js
// your custom matcher
beforeEach(function () {
    jasmine.addMatchers({
        // start writing custom matcher here
        toBePlaying: function () {
            return {
                compare: function (actual, expected) {
                    var player = actual;
                	return {
                    	pass: player.currentlyPlayingSong === expected && player.isPlaying
                	}
            	}
        	};
    	}
	});
});
```

##### Useful matchers

```js
// ===== useful matchers =====
// Check if player.currentlyPlayingSong === song
expect(player.currentlyPlayingSong).toBe(song);

// Check if player.currentlyPlayingSong equals to song. Using deep equality checking
expect(player.currentlyPlayingSong).toEqual(song);

// Check if the string is matching the regular expression supplied.
expect("A string").toMatch(/[A-za-z ]*/g);

// Check if calling resume throws a exception.
// may need to use .bind() to catch the actual Err
expect(player.resume()).toThrow();

// Check if the property abc of the player be undefined.
expect(player.abc).toBeUndefined();

// Check if the number 20 to be greater than 39
expect(20).toBeGreaterThan(39);

// Check if the number 55 is less than or equal to 100
expect(55).toBeLessThanOrEqual(100);

// Check if the PROPERTY of isPlaying is true or not.
expect(player.isPlaying).toBeTruthy();
```

##### spyOn

```js
// general
it("tells the current song if the user has made it a favorite", function() {
    spyOn(song, 'persistFavoriteStatus');
	player.play(song);
	player.makeFavorite();
	expect(song.persistFavoriteStatus).toHaveBeenCalledWith(true);
});

// ===== callthrough =====
// callthrough means the spied method will delegate your call to the real method.
// But the call is still tracked accordingly.
spyOn(song, 'persistFavoriteStatus').and.callThrough();

// ===== returnValue =====
// return a custom value when the spied method is called
spyOn(song, 'persistFavoriteStatus').and.returnValue(true);

// ===== callFake =====
// to supply a fake function to be run when the spied method is called
spyOn(song, 'persistFavoriteStatus').and.callFake(function(){
    console.log("This line will be printed when persistFavoriteStatus is called")
});

// ===== throwError =====
// throws an error when the spied method is called
spyOn(song, 'persistFavoriteStatus').and.throwError("Error is thrown");

// ===== stub =====
// default, does nothing except tracking the method call, used to return to the original
// reset function ???
spyOn(song, 'persistFavoriteStatus').and.stub();

// ===== calls =====
// use the method calls to verify the expected behaviours of the spies
it("tracks if it was called at all", function() {
    // init foo with method called setBar
    let bar = null;
    foo = {
        setBar: function(value) { bar = value; }
    };
    // spy on foo's setBar
    spyOn(foo, 'setBar');
    
    // use .calls to check if the .setBar() was called, return F since it's not called yet
    expect(foo.setBar.calls.any()).toEqual(false);

    // call .setBar()
    foo.setBar(123);
    expect(bar).toEqual(123);

    // .setBar() called, .calls() now return T
    expect(foo.setBar.calls.any()).toEqual(true);

    // return T/F
    expect(foo.setBar).toHaveBeenCalled();
    
    // return T/F
    expect(foo.setBar).toHavenBeenCalledWith(value);
    
    // stub
    foo.setBar.and.stub();
    bar = null;
    foo.setBar(123);
    expect(bar).toBe(null);
});

// ===== some more methods =====

.calls.count()
// return the number of times the spy was called.
// call be used with spyOn().stud();
	
.toHaveBeenCalledTimes(arg)
// works similar to .counts()
// but need to use .and.callThrough() on spyOn()

.calls.argsFor(i)
// return the arguments passed to ith call.

.calls.allArgs()
// return the arguments to every call in a list

.calls.all()
// return the context and the arguments passed of every call

.calls.mostRecent()
// return the context and the arguments passed to the most recent call

.calls.first()
// return the context and the arguments passed to the first call

```









