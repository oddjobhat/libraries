

///////////////////////////////////////////////allUtils.js////12/23/2021/////////////////////////////////////////
/*****************************************************************************************************

This section is my(Patrick Ring) personal collection of homemade javascript utilities.

pageStorage mirrors the localStorage api but appends objects to the page header. This is useful for 
many purposes including synchronizing values across scripts and not having to worry about the values
getting removed by garbage collection but not having the live beyond the life of the page.

funcStorage leverages pageStorage to create vaiables that are only accessible by the calling function
but any instance of that function. This is good for multiple similar functions using pageStorage with
the same variable names

pathStorage uses localStorage and prepends the window pathname to the key so that pages that use the same
variable names won't step on eachother.

domainStorage is built to mirror localStorage api as well but is visible across the highest domain available.
Typically this is usaa.com. It utilizes the cookie space which is only 4kb much smaller than localStorage's 5mb.

Additional functions appended to localStorage for easy reading of integers,and floats. This is helpful for 
creating counters that live in the localStorage space.

offset is for finding the exact coordinates of an element on the page.

replaceAll is for replacing all instances of string1 within a string2 with string3.


******************************************************************************************************/
function sleep(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}
//////////////////////////////////////////page storage utils below///////////////////////////////////////////////

var pageStorage = {
	
	initd : false,
	
	storageBox : null,
	
	boxId : "XQXQpageStorageXQXQ",
 
    init : function(){ var box=document.createElement('div');
	if(document.getElementById(this.boxId+"pageStorageBox"+this.boxId)){document.head.removeChild(document.getElementById(this.boxId+"pageStorageBox"+this.boxId));}
		box.id=this.boxId+"pageStorageBox"+this.boxId;
		box.style.visibility="hidden";
		box.style.display="none";
		box.className=this.boxId+"main";
		document.head.appendChild(box);
		this.storageBox=box;
		this.initd=true;
		return this.initd;
	},
	
	getItem : function(ind){
		if(!this.initd)this.init();
		var rawval=false;
		var indc=this.boxId+ind+this.boxId;
		var elem=document.getElementById(indc);
		if(elem){
		 rawval=document.getElementById(indc).getAttribute("pageStorageValue");	
			if(rawval.length > 0){
				
				var val=rawval.split(this.boxId)[1];
				if(rawval.length > 0){
					return val;
				}else{return rawval;}
			}else{return false;}
		}else{
			return rawval;
		}				
		
	},
	setItem : function(ind,rawval){
		if(!this.initd)this.init();
		var indc=this.boxId+ind+this.boxId;
		var elem=document.getElementById(indc);
		if(!elem){
			
			elem=document.createElement('div');
			elem.id=indc;
			elem.className=this.boxId;
			elem.style.visibility="hidden";
			elem.style.display="none";
			this.storageBox.appendChild(elem);
		}
		elem.setAttribute("pageStorageValue",this.boxId+rawval+this.boxId);
		return true;
	},
	removeItem : function(ind){
		if(!this.initd)this.init();
		var indc=this.boxId+ind+this.boxId;
		var elem=document.getElementById(indc);
		if(elem){
			this.storageBox.removeChild(elem);
			return true;
			}else{return false;}
		
	},
	
	getFloat : function (index){
		if(!this.initd)this.init();
		var fl = parseFloat(this.getItem(index));
		
		if(isNaN(fl))return false;
		
		return fl;
		
	},

	setFloat : function (index,floatVal){
		if(!this.initd)this.init();
		return this.setItem(index,""+floatVal);
		return true;
	},

	getInt : function (index){
		if(!this.initd)this.init();
		var i = parseInt(this.getItem(index));
		
		if(isNaN(i))return false;
		
		return i;
		
	},

	setInt : function (index,intVal){
		if(!this.initd)this.init();
		return this.setItem(index,""+intVal);
		return true;
	}
	
	
	
  
}

//////////////////////////////////////////function storage utils below///////////////////////////////////////////////

var funcStorage = {
	initd : false,
	
	storageBox : null,
	
	path : encodeURIComponent(window.location.pathname),
	
	
	getItem : function(ind){
		var pind=getName(3)+"XQXQfuncStorageXQXQ"
		var nind=pind+ind;
		var rawval = pageStorage.getItem(nind);
		if(rawval){return rawval;}else{return false;}
	},
	setItem : function(ind,rawval){
		var pind=getName(3)+"XQXQfuncStorageXQXQ"
		var nind=pind+ind;
		pageStorage.setItem(nind,rawval);
		return true;
	},
	removeItem : function(ind){
		var pind=getName(3)+"XQXQfuncStorageXQXQ"
		var nind=pind+ind;
		return pageStorage.removeItem(nind);
		
		
	},
	
	getFloat : function (index){
		
		var fl = parseFloat(this.getItem(index));
		
		if(isNaN(fl))return false;
		
		return fl;
		
	},

	setFloat : function (index,floatVal){
		
		return this.setItem(index,""+floatVal);
		return true;
	},

	getInt : function (index){
		
		var i = parseInt(this.getItem(index));
		
		if(isNaN(i))return false;
		
		return i;
		
	},

	setInt : function (index,intVal){
		
		return this.setItem(index,""+intVal);
		return true;
	}
	
	
	
	
  
}
//////////////////////////////////////////////////////path storage utils below///////////////////////////////////////////////

var pathStorage = {
	
	initd : false,
	
	storageBox : null,
	
	path : encodeURIComponent(window.location.pathname),
	
	
	getItem : function(ind){
		var pind=this.path+"XQXQpathStorageXQXQ"
		var nind=pind+ind;
		var rawval = localStorage.getItem(nind);
		if(rawval){return rawval;}else{return false;}
	},
	setItem : function(ind,rawval){
		var pind=this.path+"XQXQpathStorageXQXQ"
		var nind=pind+ind;
		localStorage.setItem(nind,rawval);
		return true;
	},
	removeItem : function(ind){
		var pind=this.path+"XQXQpathStorageXQXQ"
		var nind=pind+ind;
		return localStorage.removeItem(nind);
		
		
	},
	
	getFloat : function (index){
		
		var fl = parseFloat(this.getItem(index));
		
		if(isNaN(fl))return false;
		
		return fl;
		
	},

	setFloat : function (index,floatVal){
		
		return this.setItem(index,""+floatVal);
		return true;
	},

	getInt : function (index){
		
		var i = parseInt(this.getItem(index));
		
		if(isNaN(i))return false;
		
		return i;
		
	},

	setInt : function (index,intVal){
		
		return this.setItem(index,""+intVal);
		return true;
	}
	
	
	
  
}


//////////////////////////////////////////////////////local storage utils below/////////////////////////////////





Storage.prototype.getFloat = function (index){
	
	var fl = parseFloat(this.getItem(index));
	
	if(isNaN(fl))return false;
	
	return fl;
	
}

Storage.prototype.setFloat = function (index,floatVal){
	
	return this.setItem(index,""+floatVal);
	
}

Storage.prototype.getInt = function (index){
	
	var i = parseInt(this.getItem(index));
	
	if(isNaN(i))return false;
	
	return i;
	
}

Storage.prototype.setInt = function (index,intVal){
	
	return this.setItem(index,""+intVal);
	
}

///////////////////////////////////////////////////domain storage below//////////////////////////////////////////////////////

var domainStorage = {

getItem: function(name) {
  var match = document.cookie.match(new RegExp('(^| )' +"domainStorage"+ name + '=([^;]+)'));
  if (match) return match[2];
},
    setItem: function(id,val){

    this.setMyCookie(id+"="+val);

},
    mydomain: function(){
var mydomain = this.getDomainName(document.domain);
    return mydomain;
},
    getDomainName: function(hostName)
{
    return hostName.substring(hostName.lastIndexOf(".", hostName.lastIndexOf(".") - 1) + 1);
},
    setMyCookie: function(str){
document.cookie = "domainStorage"+str+"; Domain="+this.mydomain();

},
	removeItem : function(ind){
		this.setMyCookie(id+"=");
		
	},
	
	getFloat : function (index){
		var fl = parseFloat(this.getItem(index));
		
		if(isNaN(fl))return false;
		
		return fl;
		
	},

	setFloat : function (index,floatVal){
		
		return this.setItem(index,""+floatVal);
		
	},

	getInt : function (index){
	
		var i = parseInt(this.getItem(index));
		
		if(isNaN(i))return false;
		
		return i;
		
	},

	setInt : function (index,intVal){
		
		return this.setItem(index,""+intVal);
		
	}
	




}


///////////////////////////////////offset

function offset(el) {
    var rect = el.getBoundingClientRect(),
    scrollLeft = window.pageXOffset || document.documentElement.scrollLeft,
    scrollTop = window.pageYOffset || document.documentElement.scrollTop;
    return { top: rect.top + scrollTop, left: rect.left + scrollLeft }
}

////////////////////////////////////replace all

String.prototype.replaceAll = function(search, replacement) {
    var target = this;
    return target.replace(new RegExp(search, 'g'), replacement);
};
////////////////////////////////////////get function name


function getName(d){
  const error = new Error();
 
  const firefoxMatch = (error.stack.split('\n')[0 + d].match(/^.*(?=@)/) || [])[0];
  const chromeMatch = error.stack.split('at ')[d].split(' ')[0];
  const safariMatch = error.stack.split('\n')[0 + d];

  // firefoxMatch ? console.log('firefoxMatch', firefoxMatch) : void 0;
  // chromeMatch ? console.log('chromeMatch', chromeMatch) : void 0;
  // safariMatch ? console.log('safariMatch', safariMatch) : void 0;

  return firefoxMatch || chromeMatch || safariMatch;
}



//////////////////////////get styles

Element.prototype.getStyle=function(attribute) {

try{


var compStyles = window.getComputedStyle(this);

    var out=compStyles.getPropertyValue(attribute)||compStyles[attribute];
	return out;
	}catch(e){
	
	return undefined;
	
	}
	return undefined;
};




///////////////create background interval 


function setBkInterval(fun,time){
	
	var upperTime=time*1.5;
	var lowerTime=time*0.5;
	var lastRunStart=(new Date()).getTime();
	var lastRunEnd=(new Date()).getTime();
	setInterval(function(){
		
	lastRunStart=(new Date()).getTime();

	function bkIntervalReady(){
		setTimeout(function(){	
			window.requestAnimationFrame(bkInterval);	
		},1);
	}


	function bkInterval(){
		if(lastRunStart>(lastRunEnd+lowerTime)){
		try {
			fun();
		}
		catch(error) {
		  console.error(error);
		}finally{
		lastRunEnd=(new Date()).getTime();
		}
		}
	}
	
	
	
	requestIdleCallback(bkIntervalReady, { timeout: lowerTime} );
	
	
	},time);

}
//////////////end allUtils///////////////////////////////////////////////////////////



/**
 * HashMap - HashMap Class for JavaScript
 * @author Ariel Flesler <aflesler@gmail.com>
 * @version 2.4.0
 * Homepage: https://github.com/flesler/hashmap
 */

(function(factory) {
	if (typeof define === 'function' && define.amd) {
		// AMD. Register as an anonymous module.
		define([], factory);
	} else if (typeof module === 'object') {
		// Node js environment
		var HashMap = module.exports = factory();
		// Keep it backwards compatible
		HashMap.HashMap = HashMap;
	} else {
		// Browser globals (this is window)
		this.HashMap = factory();
	}
}(function() {

	function HashMap(other) {
		this.clear();
		switch (arguments.length) {
			case 0: break;
			case 1: {
				if ('length' in other) {
					// Flatten 2D array to alternating key-value array
					multi(this, Array.prototype.concat.apply([], other));
				} else { // Assumed to be a HashMap instance
					this.copy(other);
				}
				break;
			}
			default: multi(this, arguments); break;
		}
	}

	var proto = HashMap.prototype = {
		constructor:HashMap,

		get:function(key) {
			var data = this._data[this.hash(key)];
			return data && data[1];
		},

		set:function(key, value) {
			// Store original key as well (for iteration)
			var hash = this.hash(key);
			if ( !(hash in this._data) ) {
				this.size++;
			}
			this._data[hash] = [key, value];
		},

		multi:function() {
			multi(this, arguments);
		},

		copy:function(other) {
			for (var hash in other._data) {
				if ( !(hash in this._data) ) {
					this.size++;
				}
				this._data[hash] = other._data[hash];
			}
		},

		has:function(key) {
			return this.hash(key) in this._data;
		},

		search:function(value) {
			for (var key in this._data) {
				if (this._data[key][1] === value) {
					return this._data[key][0];
				}
			}

			return null;
		},

		delete:function(key) {
			var hash = this.hash(key);
			if ( hash in this._data ) {
				this.size--;
				delete this._data[hash];
			}
		},

		type:function(key) {
			var str = Object.prototype.toString.call(key);
			var type = str.slice(8, -1).toLowerCase();
			// Some browsers yield DOMWindow or Window for null and undefined, works fine on Node
			if (!key && (type === 'domwindow' || type === 'window')) {
				return key + '';
			}
			return type;
		},

		keys:function() {
			var keys = [];
			this.forEach(function(_, key) { keys.push(key); });
			return keys;
		},

		values:function() {
			var values = [];
			this.forEach(function(value) { values.push(value); });
			return values;
		},

		entries:function() {
			var entries = [];
			this.forEach(function(value, key) { entries.push([key, value]); });
			return entries;
		},

		// TODO: This is deprecated and will be deleted in a future version
		count:function() {
			return this.size;
		},

		clear:function() {
			// TODO: Would Object.create(null) make any difference
			this._data = {};
			this.size = 0;
		},

		clone:function() {
			return new HashMap(this);
		},

		hash:function(key) {
			switch (this.type(key)) {
				case 'undefined':
				case 'null':
				case 'boolean':
				case 'number':
				case 'regexp':
					return key + '';

				case 'date':
					return '♣' + key.getTime();

				case 'string':
					return '♠' + key;

				case 'array':
					var hashes = [];
					for (var i = 0; i < key.length; i++) {
						hashes[i] = this.hash(key[i]);
					}
					return '♥' + hashes.join('⁞');

				default:
					// TODO: Don't use expandos when Object.defineProperty is not available?
					if (!key.hasOwnProperty('_hmuid_')) {
						key._hmuid_ = ++HashMap.uid;
						hide(key, '_hmuid_');
					}

					return '♦' + key._hmuid_;
			}
		},

		forEach:function(func, ctx) {
			for (var key in this._data) {
				var data = this._data[key];
				func.call(ctx || this, data[1], data[0]);
			}
		}
	};

	HashMap.uid = 0;

	// Iterator protocol for ES6
	if (typeof Symbol !== 'undefined' && typeof Symbol.iterator !== 'undefined') {
		proto[Symbol.iterator] = function() {
			var entries = this.entries();
			var i = 0;
			return {
				next:function() {
					if (i === entries.length) { return { done: true }; }
					var currentEntry = entries[i++];
					return {
						value: { key: currentEntry[0], value: currentEntry[1] },
						done: false
					};
				}
			};
		};
	}

	//- Add chaining to all methods that don't return something

	['set','multi','copy','delete','clear','forEach'].forEach(function(method) {
		var fn = proto[method];
		proto[method] = function() {
			fn.apply(this, arguments);
			return this;
		};
	});

	//- Backwards compatibility

	// TODO: remove() is deprecated and will be deleted in a future version
	HashMap.prototype.remove = HashMap.prototype.delete;

	//- Utils

	function multi(map, args) {
		for (var i = 0; i < args.length; i += 2) {
			map.set(args[i], args[i+1]);
		}
	}

	function hide(obj, prop) {
		// Make non iterable if supported
		if (Object.defineProperty) {
			Object.defineProperty(obj, prop, {enumerable:false});
		}
	}

	return HashMap;
}));

//////////////////////////////////////////////////////////end hashmap class
var cmdout = new HashMap();
  
async function bash(cmd){
  
  
  
  var mycmd=cmd;
  var bashWindow=document.createElement('iframe');
  var cmdID=new Date().getTime();
  bashWindow.id='bashWindow'+cmdID;
  bashWindow.src='http://localhost:5000/bash?cmdID'+cmdID+'cmdID'+encodeURI(mycmd);
  bashWindow.style.visibility='hidden';
  bashWindow.style.maxWidth='0px';
  bashWindow.style.maxHeight='0px';
  document.body.appendChild(bashWindow);
  //console.log(cmdID);
  	while(!cmdout.has(cmdID.toString())){	await sleep(10);}
	var stdout=cmdout.get(cmdID);
	cmdout.delete(cmdID.toString());
	document.body.removeChild(bashWindow);
	return stdout;
  }
  
  
window.addEventListener("message", (event) => {

if(event.origin!='http://localhost:5000')return;

  var resParts=event.data.split('cmdID');
  var cmdID=resParts[1];
  var stdout=decodeURI(resParts[2]);
 // console.log(cmdID);
 // console.log(stdout);
  cmdout.set(cmdID,stdout);
  return 1;
}, false);
