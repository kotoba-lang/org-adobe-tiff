# kotoba-lang/org-adobe-tiff

Zero-dep portable `.cljc` implementation of TIFF 6.0, Adobe's published
raster container format spec. Named `org-adobe-tiff` (vendor-published spec,
same `org-<vendor>-<spec>` pattern as `org-synopsys-sdc`/`org-synopsys-liberty`
— a published spec document exists even though the publisher is a vendor, not
a formal standards body).

Extracted from `kotoba-lang/kasane` (kasane.tiff, ADR-2606272100). R0 extracts
IFD0 metadata (dims/bits-per-sample/compression/photometric) and decodes
pixel strips for none/LZW/PackBits compression; deflate-compressed strips are
left opaque (no external dependency wired in this repo). Carries its own
small byte-cursor and LZW/PackBits primitives rather than depending on
another kotoba-lang repo — TIFF's LZW variant (MSB-first, early code-width
change) differs from GIF's, so these aren't shared.

## Usage

```clojure
(require '[tiff.core :as tiff])

(tiff/parse tiff-bytes)    ; => {:byte-order :width :height :bits-per-sample
                            ;     :compression :photometric :entries}
(tiff/pixels tiff-bytes)   ; => vector of decoded sample bytes
```

## Test

```sh
clojure -M:test
```
