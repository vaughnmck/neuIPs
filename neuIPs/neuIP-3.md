---
eip: 3
title: Music NFT Release Event
description: A primitive to identify music NFTs on EVM chains.
author: Tim Daubenschütz (@TimDaub), Kevin Neaton (@neatonk)
discussions-to: https://github.com/neume-network/neuIPs/issues/4
status: Draft
created: 2022-09-16
---

## Abstract

We introduce a canonical event for identifying music NFTs on EVM-based chains.

## Motivation

Today, when musicians mint NFT collections on Ethereum they, unfortunately, don't signal that their NFT contains music. This is a problem as regular machines cannot trivially differentiate between a mere audio NFT and an actual music NFT.

Currently, neume identifies music NFTs by indexing collections minted through popular music NFT services. Unfortunately, this biases Neume in favour of artists that mint through a small number of supported platforms and against those that mint using their own contracts.

Artists should not be forced to mint through a supported platform to be indexed by neume.

In the following, we introduce a canoncial event for identifying an NFT that contains music.

## Specification

Through publishing this specification, the neume network community is committing itself to index all unique songs represented through music NFTS that are identified on-chain by emitting the following event:

```solidity
event NewSong(address contract, uint256 tokenId);
```

- Although a song may be represented by many tokens the `event NewSong` must only be emitted once on the network and preferrably with its lowest findable `uint256 tokenId`.
- Any `address contract` must comply with the EIP-721 or EIP-1155 Solidity interface standards or at the very least expose the `function tokenURI(uint256 tokenId) returns (string)`.
- A token's metadata, returned through `function tokenURI(uint256 tokenId) returns (string)` must comply with the following JSON schema:

```
{
  "type": "object",
  "properties": {
    "title": {
      "type": "string",
      "$comment": "The title of the song."
    },
    "artist": {
      "type": "object",
      "properties": {
        "name": {
          "type": "string",
      	  "$comment": "The song's artist."
        }
      },
      "required": [
        "name"
      ]
    },
    "manifestations": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "uri": {
            "type": "string",
            "format": "uri",
            "$comment": "A link to the song's media file."
          },
          "mimetype": {
            "$comment": "A song's mimetype.",
            "type": "string",
            "pattern": "audio|image|video|audio/1d-interleaved-parityfec|audio/32kadpcm|audio/3gpp|audio/3gpp2|audio/aac|audio/ac3|audio/adpcm|audio/amr|audio/amr-wb|audio/amr-wb+|audio/aptx|audio/asc|audio/atrac-advanced-lossless|audio/atrac-x|audio/atrac3|audio/basic|audio/bv16|audio/bv32|audio/clearmode|audio/cn|audio/dat12|audio/dls|audio/dsr-es201108|audio/dsr-es202050|audio/dsr-es202211|audio/dsr-es202212|audio/dv|audio/dvi4|audio/eac3|audio/encaprtp|audio/evrc|audio/evrc-qcp|audio/evrc0|audio/evrc1|audio/evrcb|audio/evrcb0|audio/evrcb1|audio/evrcnw|audio/evrcnw0|audio/evrcnw1|audio/evrcwb|audio/evrcwb0|audio/evrcwb1|audio/evs|audio/flexfec|audio/fwdred|audio/g711-0|audio/g719|audio/g722|audio/g7221|audio/g723|audio/g726-16|audio/g726-24|audio/g726-32|audio/g726-40|audio/g728|audio/g729|audio/g7291|audio/g729d|audio/g729e|audio/gsm|audio/gsm-efr|audio/gsm-hr-08|audio/ilbc|audio/ip-mr_v2.5|audio/isac|audio/l16|audio/l20|audio/l24|audio/l8|audio/lpc|audio/melp|audio/melp1200|audio/melp2400|audio/melp600|audio/mhas|audio/midi|audio/mobile-xmf|audio/mp3|audio/mp4|audio/mp4a-latm|audio/mpa|audio/mpa-robust|audio/mpeg|audio/mpeg4-generic|audio/musepack|audio/ogg|audio/opus|audio/parityfec|audio/pcma|audio/pcma-wb|audio/pcmu|audio/pcmu-wb|audio/prs.sid|audio/qcelp|audio/raptorfec|audio/red|audio/rtp-enc-aescm128|audio/rtp-midi|audio/rtploopback|audio/rtx|audio/s3m|audio/scip|audio/silk|audio/smv|audio/smv-qcp|audio/smv0|audio/sofa|audio/sp-midi|audio/speex|audio/t140c|audio/t38|audio/telephone-event|audio/tetra_acelp|audio/tetra_acelp_bb|audio/tone|audio/tsvcis|audio/uemclip|audio/ulpfec|audio/usac|audio/vdvi|audio/vmr-wb|audio/vnd.3gpp.iufp|audio/vnd.4sb|audio/vnd.audiokoz|audio/vnd.celp|audio/vnd.cisco.nse|audio/vnd.cmles.radio-events|audio/vnd.cns.anp1|audio/vnd.cns.inf1|audio/vnd.dece.audio|audio/vnd.digital-winds|audio/vnd.dlna.adts|audio/vnd.dolby.heaac.1|audio/vnd.dolby.heaac.2|audio/vnd.dolby.mlp|audio/vnd.dolby.mps|audio/vnd.dolby.pl2|audio/vnd.dolby.pl2x|audio/vnd.dolby.pl2z|audio/vnd.dolby.pulse.1|audio/vnd.dra|audio/vnd.dts|audio/vnd.dts.hd|audio/vnd.dts.uhd|audio/vnd.dvb.file|audio/vnd.everad.plj|audio/vnd.hns.audio|audio/vnd.lucent.voice|audio/vnd.ms-playready.media.pya|audio/vnd.nokia.mobile-xmf|audio/vnd.nortel.vbk|audio/vnd.nuera.ecelp4800|audio/vnd.nuera.ecelp7470|audio/vnd.nuera.ecelp9600|audio/vnd.octel.sbc|audio/vnd.presonus.multitrack|audio/vnd.qcelp|audio/vnd.rhetorex.32kadpcm|audio/vnd.rip|audio/vnd.rn-realaudio|audio/vnd.sealedmedia.softseal.mpeg|audio/vnd.vmx.cvsd|audio/vnd.wave|audio/vorbis|audio/vorbis-config|audio/wav|audio/wave|audio/webm|audio/x-aac|audio/x-aiff|audio/x-caf|audio/x-flac|audio/x-m4a|audio/x-matroska|audio/x-mpegurl|audio/x-ms-wax|audio/x-ms-wma|audio/x-pn-realaudio|audio/x-pn-realaudio-plugin|audio/x-realaudio|audio/x-tta|audio/x-wav|audio/xm|font/collection|font/otf|font/sfnt|font/ttf|font/woff|font/woff2|image/aces|image/apng|image/avci|image/avcs|image/avif|image/bmp|image/cgm|image/dicom-rle|image/emf|image/fits|image/g3fax|image/gif|image/heic|image/heic-sequence|image/heif|image/heif-sequence|image/hej2k|image/hsj2|image/ief|image/jls|image/jp2|image/jpeg|image/jph|image/jphc|image/jpm|image/jpx|image/jxr|image/jxra|image/jxrs|image/jxs|image/jxsc|image/jxsi|image/jxss|image/ktx|image/ktx2|image/naplps|image/pjpeg|image/png|image/prs.btif|image/prs.pti|image/pwg-raster|image/sgi|image/svg+xml|image/t38|image/tiff|image/tiff-fx|image/vnd.adobe.photoshop|image/vnd.airzip.accelerator.azv|image/vnd.cns.inf2|image/vnd.dece.graphic|image/vnd.djvu|image/vnd.dvb.subtitle|image/vnd.dwg|image/vnd.dxf|image/vnd.fastbidsheet|image/vnd.fpx|image/vnd.fst|image/vnd.fujixerox.edmics-mmr|image/vnd.fujixerox.edmics-rlc|image/vnd.globalgraphics.pgb|image/vnd.microsoft.icon|image/vnd.mix|image/vnd.mozilla.apng|image/vnd.ms-dds|image/vnd.ms-modi|image/vnd.ms-photo|image/vnd.net-fpx|image/vnd.pco.b16|image/vnd.radiance|image/vnd.sealed.png|image/vnd.sealedmedia.softseal.gif|image/vnd.sealedmedia.softseal.jpg|image/vnd.svf|image/vnd.tencent.tap|image/vnd.valve.source.texture|image/vnd.wap.wbmp|image/vnd.xiff|image/vnd.zbrush.pcx|image/webp|image/wmf|image/x-3ds|image/x-cmu-raster|image/x-cmx|image/x-freehand|image/x-icon|image/x-jng|image/x-mrsid-image|image/x-ms-bmp|image/x-pcx|image/x-pict|image/x-portable-anymap|image/x-portable-bitmap|image/x-portable-graymap|image/x-portable-pixmap|image/x-rgb|image/x-tga|image/x-xbitmap|image/x-xcf|image/x-xpixmap|image/x-xwindowdump|text/1d-interleaved-parityfec|text/cache-manifest|text/calendar|text/calender|text/cmd|text/coffeescript|text/cql|text/cql-expression|text/cql-identifier|text/css|text/csv|text/csv-schema|text/directory|text/dns|text/ecmascript|text/encaprtp|text/enriched|text/fhirpath|text/flexfec|text/fwdred|text/gff3|text/grammar-ref-list|text/html|text/jade|text/javascript|text/jcr-cnd|text/jsx|text/less|text/markdown|text/mathml|text/mdx|text/mizar|text/n3|text/parameters|text/parityfec|text/plain|text/provenance-notation|text/prs.fallenstein.rst|text/prs.lines.tag|text/prs.prop.logic|text/raptorfec|text/red|text/rfc822-headers|text/richtext|text/rtf|text/rtp-enc-aescm128|text/rtploopback|text/rtx|text/sgml|text/shaclc|text/shex|text/slim|text/spdx|text/strings|text/stylus|text/t140|text/tab-separated-values|text/troff|text/turtle|text/ulpfec|text/uri-list|text/vcard|text/vnd.a|text/vnd.abc|text/vnd.ascii-art|text/vnd.curl|text/vnd.curl.dcurl|text/vnd.curl.mcurl|text/vnd.curl.scurl|text/vnd.debian.copyright|text/vnd.dmclientscript|text/vnd.dvb.subtitle|text/vnd.esmertec.theme-descriptor|text/vnd.familysearch.gedcom|text/vnd.ficlab.flt|text/vnd.fly|text/vnd.fmi.flexstor|text/vnd.gml|text/vnd.graphviz|text/vnd.hans|text/vnd.hgl|text/vnd.in3d.3dml|text/vnd.in3d.spot|text/vnd.iptc.newsml|text/vnd.iptc.nitf|text/vnd.latex-z|text/vnd.motorola.reflex|text/vnd.ms-mediapackage|text/vnd.net2phone.commcenter.command|text/vnd.radisys.msml-basic-layout|text/vnd.senx.warpscript|text/vnd.si.uricatalogue|text/vnd.sosi|text/vnd.sun.j2me.app-descriptor|text/vnd.trolltech.linguist|text/vnd.wap.si|text/vnd.wap.sl|text/vnd.wap.wml|text/vnd.wap.wmlscript|text/vtt|text/x-asm|text/x-c|text/x-component|text/x-fortran|text/x-gwt-rpc|text/x-handlebars-template|text/x-java-source|text/x-jquery-tmpl|text/x-lua|text/x-markdown|text/x-nfo|text/x-opml|text/x-org|text/x-pascal|text/x-processing|text/x-sass|text/x-scss|text/x-setext|text/x-sfv|text/x-suse-ymp|text/x-uuencode|text/x-vcalendar|text/x-vcard|text/xml|text/xml-external-parsed-entity|text/yaml|video/1d-interleaved-parityfec|video/3gpp|video/3gpp-tt|video/3gpp2|video/av1|video/bmpeg|video/bt656|video/celb|video/dv|video/encaprtp|video/ffv1|video/flexfec|video/h261|video/h263|video/h263-1998|video/h263-2000|video/h264|video/h264-rcdo|video/h264-svc|video/h265|video/iso.segment|video/jpeg|video/jpeg2000|video/jpm|video/jxsv|video/mj2|video/mp1s|video/mp2p|video/mp2t|video/mp4|video/mp4v-es|video/mpeg|video/mpeg4-generic|video/mpv|video/nv|video/ogg|video/parityfec|video/pointer|video/quicktime|video/raptorfec|video/raw|video/rtp-enc-aescm128|video/rtploopback|video/rtx|video/scip|video/smpte291|video/smpte292m|video/ulpfec|video/vc1|video/vc2|video/vnd.cctv|video/vnd.dece.hd|video/vnd.dece.mobile|video/vnd.dece.mp4|video/vnd.dece.pd|video/vnd.dece.sd|video/vnd.dece.video|video/vnd.directv.mpeg|video/vnd.directv.mpeg-tts|video/vnd.dlna.mpeg-tts|video/vnd.dvb.file|video/vnd.fvt|video/vnd.hns.video|video/vnd.iptvforum.1dparityfec-1010|video/vnd.iptvforum.1dparityfec-2005|video/vnd.iptvforum.2dparityfec-1010|video/vnd.iptvforum.2dparityfec-2005|video/vnd.iptvforum.ttsavc|video/vnd.iptvforum.ttsmpeg2|video/vnd.motorola.video|video/vnd.motorola.videop|video/vnd.mpegurl|video/vnd.ms-playready.media.pyv|video/vnd.nokia.interleaved-multimedia|video/vnd.nokia.mp4vr|video/vnd.nokia.videovoip|video/vnd.objectvideo|video/vnd.radgamettools.bink|video/vnd.radgamettools.smacker|video/vnd.sealed.mpeg1|video/vnd.sealed.mpeg4|video/vnd.sealed.swf|video/vnd.sealedmedia.softseal.mov|video/vnd.uvvu.mp4|video/vnd.vivo|video/vnd.youtube.yt|video/vp8|video/vp9|video/webm|video/x-f4v|video/x-fli|video/x-flv|video/x-m4v|video/x-matroska|video/x-mng|video/x-ms-asf|video/x-ms-vob|video/x-ms-wm|video/x-ms-wmv|video/x-ms-w"
          }
        },
        "required": [
          "uri",
          "mimetype"
        ]
      },
      "contains": {
        "type": "object",
        "properties": {
          "mimetype": {
            "type": "string",
            "pattern": "audio"
	    "$comment": "While a song's album art isn't strictly required, a song's audio file is."
          }
        }
      }
    }
  },
  "required": [
    "manifestations",
    "title",
    "artist",
  ]
}
```

## Rationale

The neume network is based on an extract, transform and load model where data source extraction is separated from transforming returned payloads to minimize the network-based tasks which have the highest risk of failure and take the longest to complete.

Still, it's uneconomical for the neume network developers to browse through all NFTs only to find individual ones to index. Particularly when it then also requires them to write custom JSON object transformers.

We've hence decided to propose this standard for signaling new music NFT releases and we mandate a minimum viable JSON schema. And to return the favor to artists and their smart contract operators, upon compliance with the above standard, we're committing ourselves to optimistically index all unique songs on all EVM-based chains emitting the above event.

## Backwards Compatibility

There are a number of music NFTs already minted with immutable metadata that won't be changable to comply with the above definitions. They may, however, be still submitted as `event NewSong(...)` releases but may not be picked up by the neume network in the first iterations of this standard's implementation. Over time, however, if yet a different but frequently-used metadata schema emerges, there is a good chance that the neume network developers are implementing its transform steps too.

## Security Considerations

There are no security considerations related directly to the implementation of this standard.

## Copyright

Copyright and related rights waived via [CC0](../LICENSE.md).
