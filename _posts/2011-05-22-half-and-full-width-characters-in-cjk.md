---
layout: post
title: Half- And Full-width Characters In CJK (and normalization)
date: '2011-05-22T04:22:00.000-07:00'
categories: programming
author: Sully
tags:
- ASCII
- Java
- Mapping of Unicode characters
- UTF-8
- Unicode
- OSI protocols
- Character encoding
- Fullwidth form
- Half-width kana
- preferable normalization solution
- Character sets
- Notation
- Chinese
- CJK characters
- Kana
- Typography
- Korean
- Japanese
modified_time: '2015-01-02T23:55:54.101-08:00'
blogger_id: tag:blogger.com,1999:blog-7835395644559909128.post-65491526938295944
blogger_orig_url: http://blog.solutions.asia/2011/05/half-and-full-width-characters-in-cjk.html
---
## CHARACTER SIZES
<div style="text-align: left;">Latin Characters in half-width and full-width</div>
<br/>
<div style="text-align: center;">Asia Ａｓｉａ</div>
<br/>
<div>Katakana in half-width and full-width</div>
<br/>
<div style="text-align: center;">ｱｼﾞｱ アジア</div>
<br/>
<div>Kanji full-width only</div>
<br/>
<div style="text-align: center;">亜</div>
<br/>
<div>The terms half- and full-width refer to the relative width size of the glyph of characters and is important to CJK languages as most of their characters require full width characters to display due to their complexity. The origin of half width characters goes back to the early days of computing when single byte representation of characters was the norm and Japanese and Korean computer manufacturers displayed very limited ranges of their languages’ characters in low resolution in single byte encodings. In fact some people still refer to half width characters as single byte characters and full width characters as double-byte characters but this should be discouraged as this has not necessarily been true for some time—half width characters may be encoded using multiple bytes and full width characters may be encoded using more than two bytes depending on the encoding scheme.</div>
<br/>
<div>As exciting as the history of byte encoding is we are not going to cover it here. Suffice it say that size of characters is not something that should be handled by character code mapping but something that belongs in font, style sheets, or other display markup and the only reason this issue is still with us today is that for backwards compatibility reasons, the Unicode Consortium decided to include the half-width/full width character distinction in their mappings. So instead of taking some admittedly significant pain around conversion five or ten years ago, we are going to have to deal with the half- and full-width issue for the foreseeable future. This is surprising considering the outstanding job of rationalization and simplification that the Unicode consortium has managed to pull off in almost all other areas of the extremely complicated subject of CJK character encoding.</div>
<br/>
### What’s the issue?
<div>Why is half- and full-width such an issue? The problem is that having the character width distinction handled in character code mapping means all CJK text needs to be normalized before searching, sorting, filtering, matching, etc. or it will not return the expected results. People searching for a word expect all results to be returned not just those that are in the half-width format that they may have inputted in their search query or vice versa if they inputted their search term in full-width format.</div>
<br/>
### Chinese, Japanese and Korean
<div>In Chinese, half and full-width are usually referred to as 半角（bànjiǎo) and 全角（quánjiǎo） in mainland China and 半形 ( bànxíng) and 全形 (quánxíng) in Taiwan. In Japanese, these terms are referred to as 半角 (hankaku) and 全角 (zenkaku). In Korean, the terms 반각 (bangak) and 전각 (jeongak) are used.</div>
<br/>
<div>Half- and full-width distinction is less of an issue for Chinese than Japanese and Korean because it only applies to Latin characters, symbols, and numbers for Chinese. In the case of Japanese and Korean, the distinction also applies to Katakana (Japanese) and Hangul (Korean) characters, in addition to Latin characters, numbers, etc.</div>
<br/>
### Relevant Unicode Ranges from Unicode 6.0
<div>The width size column in the table below is a gross simplification but is sufficient for our purposes. Those interested in understanding East Asian Width in more detail should refer to <a href="http://unicode.org/reports/tr11/">Unicode Standard Annex #11</a>.</div>
<br/>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
   <tbody>
      <tr>
         <td width="19%"><strong>Range</strong></td>
         <td width="59%"><strong>Content </strong></td>
         <td width="21%"><strong>Width Size</strong></td>
      </tr>
      <tr>
         <td width="19%">0x0020-0x007F</td>
         <td width="59%">
            <div>ASCII (Latin characters, symbols, punctuation,</div>
            <div>numbers)</div>
         </td>
         <td valign="top" width="21%">HALF-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x1100-0x11FF</td>
         <td width="59%">
            <div>Hangul Jamo</div>
            <div>(Korean)</div>
         </td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x3000-0x303F</td>
         <td width="59%">CJK punctuation</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x3040-0x309F</td>
         <td width="59%">Hiragana (Japanese)</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x30A0-0x30FF</td>
         <td width="59%">Katakana (Japanese)</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x3130-0x318F</td>
         <td width="59%">Hangul Compatibility (Korean for KS X1001 compatibility)</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0xAC00-0xD7AC</td>
         <td width="59%">Hangul Syllables</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0xF900-0xFAFF</td>
         <td width="59%">CJK Compatibility Ideographs Block</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0xFF00-0xFFEF</td>
         <td width="59%">
            <div><a href="http://unicode.org/charts/PDF/UFF00.pdf">Full-width</a></div>
            &nbsp;
            <div>Latin characters and half-width Katakana and Hangul</div>
            &nbsp;
         </td>
         <td valign="top" width="21%">HALF-WIDTH AND FULL WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x4E00-0x9FFF</td>
         <td width="59%">CJK unifed ideographs - Common and uncommon</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x3400-0x4DBF</td>
         <td width="59%">CJK unified ideographs Extension A - Rare</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
      <tr>
         <td width="19%">0x20000-0x2FFF</td>
         <td width="59%">CJK unified ideographs Extension B - Very rare</td>
         <td valign="top" width="21%">FULL-WIDTH</td>
      </tr>
   </tbody>
</table>
<br/>
## NORMALIZATION
<div>The next question is how to best normalize text to ignore the differences between half-width and full-width forms when doing searches, sorts, etc.</div>
<br/>
<div>Consulting the table from the previous chapter shows that there is always a full-width version but not always a half-width version of characters so at first thought it might seem easiest to just convert everything into full-width size for normalization.</div>
<br/>
<div>However, that is probably not the best solution for two reasons. A large amount of software for English and other languages using the Latin character set only targets the character codes for the ASCII half width versions and that software would all have to be rewritten. The second is that Latin characters generally look ugly in full width form.</div>
<br/>
<div>For those reasons the preferable normalization solution is to convert all Latin characters, symbols, punctuation and numbers to half-width ASCII form and everything else to their full-width form.</div>
<br/>
<div>Example code in Java for half and full-width normalization is included in an appendix at the end (note Java has this functionality built in but I have included this code for explanatory purposes). This code is simple and can easily be implemented in different computer languages that do not have Unicode Normalization built-in.</div>
<br/>
<div>The <a href="http://www.unicode.org/reports/tr15/">Unicode Standard Annex #15</a> defines normalization forms for Unicode text (covers more than just half- and full-width normalization) and without going into the details the Unicode Standardization Form most useful for normalization of searches, etc. is NFKC as both half- and full-width differences are normalized and different compatibility equivalents of a single CJK character will result in the same string.</div>
<br>
### Built-in Normalization
<div>Many computer languages and applications have the needed normalization built-in. For example in Java, which has Unicode Normalization Form functionality built in it is as easy as</div>
<br/>
<div><code>String normalized_string = java.text.Normalizer.normalize(unnormalized_string, java.text.Normalizer.Form.NFKC);</code></div>
<br/>
<div>Microsoft offers a similar functionality for Unicode normalization of strings. IBM products such as search offer equivalent normalization. For example, in Japanese, a full-width alphanumeric character is normalized to the half-width character, a half-width Katakana character to the full-width character, and so on.</div>
<br/>
### Complexity of Normalization
<div>Normalization of Unicode can be quite complex. Take for example the simple space character with all its different variants in Unicode.</div>
<br/>
<div>ASCII 0020  SPACE</div>
<br/>
<div>sometimes considered a control code</div>
<br/>
<div>other space characters: 2000  –200A </div>
<br/>
<div>→ 00A0  no-break space</div>
<br/>
<div>→ 200B  zero width space</div>
<br/>
<div>→ 2060  word joiner</div>
<br/>
<div>→ 3000  ideographic space</div>
<br/>
<div>→ FEFF  zero width no-break space</div>
<br/>
<div>For this, and a large number of other reasons I <strong>strongly recommend using the Unicode NFKC normalized format</strong> from a standard library whenever possible. Where this is not possible please see the code sample below.</div>
<br/>
<div style="text-align: center;"><span style="font-family: Times New Roman; font-size: small;"><span style="font-size: small;"><strong>JAVA CODE</strong></span></span></div>
<br/>
```java
import java.util.Map;
import java.util.HashMap;
 
/**
 * Halfwidth and Fullwidth Character Normalization for CJK
 * http://solutions.asia
 *
 * See the Unicode Standard 6.0 – Halfwidth and Fullwidth Forms
 * http://unicode.org/charts/PDF/UFF00.pdf
 *
 * For Chinese, Japanese and Korean, some characters have Unicode mappings to
 * both a halfwidth and a fullwidth version. This code normalizes them
 * to halfwidth for latin characters, numbers and punctuation and fullwidth
 * for everything else.
 * Fine for half/full width normalization but not fully equivalent to NFKC
 * normalization
 */
public class CJKHalfFullWidthNormalize {
   
    private static final Map<Character, Character> charCodeMap;
    // Key — Original Character
    // Value — Replacement character
    static {
        charCodeMap = new HashMap<Character, Character>();
        // TO HALFWIDTH CHARACTERS
        // ASCII variants (Latin Symbols, Punctuation, Numbers, and Alphabet)
        for (char key = '\uff01'; key <= '\uff5e'; key++) {
            char value = (char) (key - '\ufee0');
            charCodeMap.put(key, value);
        }
        // Brackets
        charCodeMap.put('\uff5f', '\u2985'); // left white parenthesis
        charCodeMap.put('\uff60', '\u2986'); // right white parenthesis
        // Symbol Variants
        charCodeMap.put('\uffe0', '\u00a2'); // Cent sign
        charCodeMap.put('\uffe1', '\u00a3'); // Pound sign
        charCodeMap.put('\uffe2', '\u00ac'); // Not sign
        charCodeMap.put('\uffe3', '\u00af'); // Macron
        charCodeMap.put('\uffe4', '\u00a6'); // Broken Bar
        charCodeMap.put('\uffe5', '\u00a5'); // Yen sign
        charCodeMap.put('\uffe6', '\u20a9'); // Won sign
        // Space (strictly speaking not listed in Unicode 6.0 Halfwidth and
        // Fullwidth forms but including here as the ideographic space can
        // cause issues)
        charCodeMap.put('\u3000', '\u0020'); // SPACE
        // TO FULLWIDTH CHARACTERS
        // CJK punctuation
        charCodeMap.put('\uff61', '\u3002'); // ideographic full stop
        charCodeMap.put('\uff62', '\u300c'); // left corner bracket
        charCodeMap.put('\uff63', '\u300d'); // right corner bracket
        charCodeMap.put('\uff64', '\u3001'); // ideographic comma
        // Katakana variants
        charCodeMap.put('\uff65', '\u30fb'); // Middle Dot
        charCodeMap.put('\uff66', '\u30f2'); // Wo
        charCodeMap.put('\uff67', '\u30a1'); // A small
        charCodeMap.put('\uff68', '\u30a3'); // I small
        charCodeMap.put('\uff69', '\u30a5'); // U small
        charCodeMap.put('\uff6a', '\u30a7'); // E small
        charCodeMap.put('\uff6b', '\u30a9'); // O small
        charCodeMap.put('\uff6c', '\u30e3'); // Ya small
        charCodeMap.put('\uff6d', '\u30e5'); // Yu small
        charCodeMap.put('\uff6e', '\u30e7'); // Yo small
        charCodeMap.put('\uff6f', '\u30c3'); // Tsu small
        charCodeMap.put('\uff70', '\u30fc'); // Prolonged Sound Mark
        charCodeMap.put('\uff71', '\u30a2'); // A
        charCodeMap.put('\uff72', '\u30a4'); // I
        charCodeMap.put('\uff73', '\u30a6'); // U
        charCodeMap.put('\uff74', '\u30a8'); // E
        charCodeMap.put('\uff75', '\u30aa'); // O
        charCodeMap.put('\uff76', '\u30ab'); // Ka
        charCodeMap.put('\uff77', '\u30ad'); // Ki
        charCodeMap.put('\uff78', '\u30af'); // Ku
        charCodeMap.put('\uff79', '\u30b1'); // Ke
        charCodeMap.put('\uff7a', '\u30b3'); // Ko
        charCodeMap.put('\uff7b', '\u30b5'); // Sa
        charCodeMap.put('\uff7c', '\u30b7'); // Shi
        charCodeMap.put('\uff7d', '\u30b9'); // Su
        charCodeMap.put('\uff7e', '\u30bb'); // Se
        charCodeMap.put('\uff7f', '\u30bd'); // So
        charCodeMap.put('\uff80', '\u30bf'); // Ta
        charCodeMap.put('\uff81', '\u30c1'); // Chi
        charCodeMap.put('\uff82', '\u30c4'); // Tsu
        charCodeMap.put('\uff83', '\u30c6'); // Te
        charCodeMap.put('\uff84', '\u30c8'); // To
        charCodeMap.put('\uff85', '\u30ca'); // Na
        charCodeMap.put('\uff86', '\u30cb'); // Ni
        charCodeMap.put('\uff87', '\u30cc'); // Nu
        charCodeMap.put('\uff88', '\u30cd'); // Ne
        charCodeMap.put('\uff89', '\u30ce'); // No
        charCodeMap.put('\uff8a', '\u30cf'); // Ha
        charCodeMap.put('\uff8b', '\u30d2'); // Hi
        charCodeMap.put('\uff8c', '\u30d5'); // Hu
        charCodeMap.put('\uff8d', '\u30d8'); // He
        charCodeMap.put('\uff8e', '\u30db'); // Ho
        charCodeMap.put('\uff8f', '\u30de'); // Ma
        charCodeMap.put('\uff90', '\u30df'); // Mi
        charCodeMap.put('\uff91', '\u30e0'); // Mu
        charCodeMap.put('\uff92', '\u30e1'); // Me
        charCodeMap.put('\uff93', '\u30e2'); // Mo
        charCodeMap.put('\uff94', '\u30e4'); // Ya
        charCodeMap.put('\uff95', '\u30e6'); // Yu
        charCodeMap.put('\uff96', '\u30e8'); // Yo
        charCodeMap.put('\uff97', '\u30e9'); // Ra
        charCodeMap.put('\uff98', '\u30ea'); // Ri
        charCodeMap.put('\uff99', '\u30eb'); // Ru
        charCodeMap.put('\uff9a', '\u30ec'); // Re
        charCodeMap.put('\uff9b', '\u30ed'); // Ro
        charCodeMap.put('\uff9c', '\u30ef'); // Wa
        charCodeMap.put('\uff9d', '\u30f3'); // N
        charCodeMap.put('\uff9e', '\u3099'); // Voiced Sound Mark
        charCodeMap.put('\uff9f', '\u309a'); // Semi-Voiced Sound Mark
        // Hangul variants
        charCodeMap.put('\uffa0', '\u3164'); // Hangul Filler
        // Hangul First Range
        // KIYEOK to HIEUH
        for (char key = '\uffa1'; key <= '\uffbe'; key++) {
            char value = (char) (key - '\uce70');
            charCodeMap.put(key, value);
        }
        // Hangul Second Range
        // A to E
        for (char key = '\uffc2'; key <= '\uffc7'; key++) {
            char value = (char) (key - '\uce73');
            charCodeMap.put(key, value);
        }
        // Hangul Third Range
        // YEO to OE
        for (char key = '\uffca'; key <= '\uffcf'; key++) {
            char value = (char) (key - '\uce75');
            charCodeMap.put(key, value);
        }
        // Hangul Fourth Range
        // YO to YU
        for (char key = '\uffd2'; key <= '\uffd7'; key++) {
            char value = (char) (key - '\uce77');
            charCodeMap.put(key, value);
        }
        // More Hangul variants
        charCodeMap.put('\uffda', '\u3161'); // Hangul EU
        charCodeMap.put('\uffdb', '\u3162'); // Hangul YI
        charCodeMap.put('\uffdc', '\u3163'); // Hangul I
        // Symbol Variants
        charCodeMap.put('\uffe8', '\u2502'); // Forms Light Vertical
        charCodeMap.put('\uffe9', '\u2190'); // Leftwards Arrow
        charCodeMap.put('\uffea', '\u2191'); // Upwards Arrow
        charCodeMap.put('\uffeb', '\u2192'); // Rightwards Arrow
        charCodeMap.put('\uffec', '\u2193'); // Downwards Arrow
        charCodeMap.put('\uffed', '\u25a0'); // Black Square
        charCodeMap.put('\uffee', '\u25cb'); // White Circle
    }
   
    /**
     * Takes an unnormalized (Halfwidth/Fullwidth) and outputs a normalized string
     */
    public static void main(String[] args) {
        String unnormalized = args[0];
        System.out.println("Unnormalized:\t " + unnormalized);
        char[] buffer = unnormalized.toCharArray();
        int bufferLen = buffer.length;
        for (int i = 0; i < bufferLen; i++) {
            if (charCodeMap.containsKey(buffer[i])) {
                buffer[i] = charCodeMap.get(buffer[i]);
            }
        }
        System.out.println("Normalized:\t " + new String(buffer));
    }
   
}
```