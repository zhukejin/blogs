扩展 js 函数
--

### 字符串自动加反斜杠

	String.prototype.addslashes = function (){
	    return (this + '')
	     .replace(/[\\"']/g, '\\$&')
	     .replace(/\u0000/g, '\\0');
	}

使用方法：

	console.log("asd'".addslashes()); // asd\'

### 去掉反斜杠

	/**
	 * [addcslashes 去掉反斜杠]
	 * @param  {[string]} charlist [addcslashes() 影响的字符或字符范围]
	 * @return {[string]}          [处理后的字符串]
	 */
	String.prototype.addcslashes = function (charlist) {
	    var target = '',
	      chrs = [],
	      i = 0,
	      j = 0,
	      c = '',
	      next = '',
	      rangeBegin = '',
	      rangeEnd = '',
	      chr = '',
	      begin = 0,
	      end = 0,
	      octalLength = 0,
	      postOctalPos = 0,
	      cca = 0,
	      escHexGrp = [],
	      encoded = '',
	      percentHex = /%([\dA-Fa-f]+)/g;
	    var _pad = function(n, c) {
	      if ((n = n + '')
	        .length < c) {
	        return new Array(++c - n.length)
	          .join('0') + n;
	      }
	      return n;
	    };
	
	    for (i = 0; i < charlist.length; i++) {
	      c = charlist.charAt(i);
	      next = charlist.charAt(i + 1);
	      if (c === '\\' && next && (/\d/)
	        .test(next)) { // Octal
	        rangeBegin = charlist.slice(i + 1)
	          .match(/^\d+/)[0];
	        octalLength = rangeBegin.length;
	        postOctalPos = i + octalLength + 1;
	        if (charlist.charAt(postOctalPos) + charlist.charAt(postOctalPos + 1) === '..') {
	          begin = rangeBegin.charCodeAt(0);
	          if ((/\\\d/)
	            .test(charlist.charAt(postOctalPos + 2) + charlist.charAt(postOctalPos + 3))) {
	            rangeEnd = charlist.slice(postOctalPos + 3)
	              .match(/^\d+/)[0];
	            i += 1; // Skip range end backslash
	          } else if (charlist.charAt(postOctalPos + 2)) {
	            rangeEnd = charlist.charAt(postOctalPos + 2);
	          } else {
	            throw 'Range with no end point';
	          }
	          end = rangeEnd.charCodeAt(0);
	          if (end > begin) {
	            for (j = begin; j <= end; j++) {
	              chrs.push(String.fromCharCode(j));
	            }
	          } else {
	            chrs.push('.', rangeBegin, rangeEnd);
	          }
	          i += rangeEnd.length + 2;
	        } else {
	          chr = String.fromCharCode(parseInt(rangeBegin, 8));
	          chrs.push(chr);
	        }
	        i += octalLength;
	      } else if (next + charlist.charAt(i + 2) === '..') {
	        rangeBegin = c;
	        begin = rangeBegin.charCodeAt(0);
	        if ((/\\\d/)
	          .test(charlist.charAt(i + 3) + charlist.charAt(i + 4))) {
	          rangeEnd = charlist.slice(i + 4)
	            .match(/^\d+/)[0];
	          i += 1;
	        } else if (charlist.charAt(i + 3)) {
	          rangeEnd = charlist.charAt(i + 3);
	        } else {
	          throw 'Range with no end point';
	        }
	        end = rangeEnd.charCodeAt(0);
	        if (end > begin) {
	          for (j = begin; j <= end; j++) {
	            chrs.push(String.fromCharCode(j));
	          }
	        } else {
	          chrs.push('.', rangeBegin, rangeEnd);
	        }
	        i += rangeEnd.length + 2;
	      } else {
	        chrs.push(c);
	      }
	    }
	
	    for (i = 0; i < this.length; i++) {
	      c = this.charAt(i);
	      if (chrs.indexOf(c) !== -1) {
	        target += '\\';
	        cca = c.charCodeAt(0);
	        if (cca < 32 || cca > 126) {
	          switch (c) {
	            case '\n':
	              target += 'n';
	              break;
	            case '\t':
	              target += 't';
	              break;
	            case '\u000D':
	              target += 'r';
	              break;
	            case '\u0007':
	              target += 'a';
	              break;
	            case '\v':
	              target += 'v';
	              break;
	            case '\b':
	              target += 'b';
	              break;
	            case '\f':
	              target += 'f';
	              break;
	            default:
	              //target += _pad(cca.toString(8), 3);break; // Sufficient for UTF-16
	              encoded = encodeURIComponent(c);
	
	              // 3-length-padded UTF-8 octets
	              if ((escHexGrp = percentHex.exec(encoded)) !== null) {
	                target += _pad(parseInt(escHexGrp[1], 16)
	                  .toString(8), 3); // already added a slash above
	              }
	              while ((escHexGrp = percentHex.exec(encoded)) !== null) {
	                target += '\\' + _pad(parseInt(escHexGrp[1], 16)
	                  .toString(8), 3);
	              }
	              break;
	          }
	        } else { // Perform regular backslashed escaping
	          target += c;
	        }
	      } else { // Just add the character unescaped
	        target += c;
	      }
	    }
	    return target;
	  
	}
	
	
	console.log("asd\'".addcslashes('a')) //在 a前面加反斜杠



### sha1（）

		function sha1(str) {
	  //  discuss at: http://phpjs.org/functions/sha1/
	  // original by: Webtoolkit.info (http://www.webtoolkit.info/)
	  // improved by: Michael White (http://getsprink.com)
	  // improved by: Kevin van Zonneveld (http://kevin.vanzonneveld.net)
	  //    input by: Brett Zamir (http://brett-zamir.me)
	  //  depends on: utf8_encode
	  //   example 1: sha1('Kevin van Zonneveld');
	  //   returns 1: '54916d2e62f65b3afa6e192e6a601cdbe5cb5897'
	
	  var rotate_left = function(n, s) {
	    var t4 = (n << s) | (n >>> (32 - s));
	    return t4;
	  };
	
	  /*var lsb_hex = function (val) { // Not in use; needed?
	    var str="";
	    var i;
	    var vh;
	    var vl;
	
	    for ( i=0; i<=6; i+=2 ) {
	      vh = (val>>>(i*4+4))&0x0f;
	      vl = (val>>>(i*4))&0x0f;
	      str += vh.toString(16) + vl.toString(16);
	    }
	    return str;
	  };*/
	
	  var cvt_hex = function(val) {
	    var str = '';
	    var i;
	    var v;
	
	    for (i = 7; i >= 0; i--) {
	      v = (val >>> (i * 4)) & 0x0f;
	      str += v.toString(16);
	    }
	    return str;
	  };
	
	  var blockstart;
	  var i, j;
	  var W = new Array(80);
	  var H0 = 0x67452301;
	  var H1 = 0xEFCDAB89;
	  var H2 = 0x98BADCFE;
	  var H3 = 0x10325476;
	  var H4 = 0xC3D2E1F0;
	  var A, B, C, D, E;
	  var temp;
	
	  str = this.utf8_encode(str);
	  var str_len = str.length;
	
	  var word_array = [];
	  for (i = 0; i < str_len - 3; i += 4) {
	    j = str.charCodeAt(i) << 24 | str.charCodeAt(i + 1) << 16 | str.charCodeAt(i + 2) << 8 | str.charCodeAt(i + 3);
	    word_array.push(j);
	  }
	
	  switch (str_len % 4) {
	    case 0:
	      i = 0x080000000;
	      break;
	    case 1:
	      i = str.charCodeAt(str_len - 1) << 24 | 0x0800000;
	      break;
	    case 2:
	      i = str.charCodeAt(str_len - 2) << 24 | str.charCodeAt(str_len - 1) << 16 | 0x08000;
	      break;
	    case 3:
	      i = str.charCodeAt(str_len - 3) << 24 | str.charCodeAt(str_len - 2) << 16 | str.charCodeAt(str_len - 1) <<
	        8 | 0x80;
	      break;
	  }
	
	  word_array.push(i);
	
	  while ((word_array.length % 16) != 14) {
	    word_array.push(0);
	  }
	
	  word_array.push(str_len >>> 29);
	  word_array.push((str_len << 3) & 0x0ffffffff);
	
	  for (blockstart = 0; blockstart < word_array.length; blockstart += 16) {
	    for (i = 0; i < 16; i++) {
	      W[i] = word_array[blockstart + i];
	    }
	    for (i = 16; i <= 79; i++) {
	      W[i] = rotate_left(W[i - 3] ^ W[i - 8] ^ W[i - 14] ^ W[i - 16], 1);
	    }
	
	    A = H0;
	    B = H1;
	    C = H2;
	    D = H3;
	    E = H4;
	
	    for (i = 0; i <= 19; i++) {
	      temp = (rotate_left(A, 5) + ((B & C) | (~B & D)) + E + W[i] + 0x5A827999) & 0x0ffffffff;
	      E = D;
	      D = C;
	      C = rotate_left(B, 30);
	      B = A;
	      A = temp;
	    }
	
	    for (i = 20; i <= 39; i++) {
	      temp = (rotate_left(A, 5) + (B ^ C ^ D) + E + W[i] + 0x6ED9EBA1) & 0x0ffffffff;
	      E = D;
	      D = C;
	      C = rotate_left(B, 30);
	      B = A;
	      A = temp;
	    }
	
	    for (i = 40; i <= 59; i++) {
	      temp = (rotate_left(A, 5) + ((B & C) | (B & D) | (C & D)) + E + W[i] + 0x8F1BBCDC) & 0x0ffffffff;
	      E = D;
	      D = C;
	      C = rotate_left(B, 30);
	      B = A;
	      A = temp;
	    }
	
	    for (i = 60; i <= 79; i++) {
	      temp = (rotate_left(A, 5) + (B ^ C ^ D) + E + W[i] + 0xCA62C1D6) & 0x0ffffffff;
	      E = D;
	      D = C;
	      C = rotate_left(B, 30);
	      B = A;
	      A = temp;
	    }
	
	    H0 = (H0 + A) & 0x0ffffffff;
	    H1 = (H1 + B) & 0x0ffffffff;
	    H2 = (H2 + C) & 0x0ffffffff;
	    H3 = (H3 + D) & 0x0ffffffff;
	    H4 = (H4 + E) & 0x0ffffffff;
	  }
	
	  temp = cvt_hex(H0) + cvt_hex(H1) + cvt_hex(H2) + cvt_hex(H3) + cvt_hex(H4);
	  return temp.toLowerCase();
	}