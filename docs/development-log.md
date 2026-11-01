# Development and verification log

## 2026-11-01 — Requirements and source verification

- Scope: one document conversion skill, Rust anydoc, Windows/macOS, file/directory input, auditable per-file Markdown bundles. Wiki synthesis and OKF compliance are outside this skill.
- Latest stable verified: anydoc 0.2.4; release tag commit `42bf1c5ecdde9eb0d96d6bd75a9e6698cf93b14c`, published 2026-08-27; crate checksum `cf0d78e4cfe3654eb3ea04422ec5a94352000ee56e97902125e68e51f7527062`.
- Critical API gap: public `to_document` exposes assets but public `to_markdown_bytes` does not render a modified Document. Vendor only the matching MIT Markdown serializer, then replace embedded image references with safe local asset paths before rendering. No parser fork.
- Independent probe compiled the serializer against the published crate and matched official RTF output byte for byte.
- Downloads inventory found real DOCX/CSV/PDF/EPUB inputs. Missing parser families are supplemented with official MIT fixtures. Originals, extracted content, and private paths stay outside tracked files.
- Evaluation plan: Plugin Eval static analysis and recommendations, official Agent Skills validator, Rust tests, independent forward task, real corpus checks, and native Windows/macOS CI. Static scores do not establish conversion accuracy.

## Implementation and observed iteration

- Implemented per-file bundles, original snapshots, UTF-8/LF Markdown, flat provenance, local assets, manifests, failure pages and read-only artifact verification. Unknown/corrupt inputs do not prevent remaining files from being processed.
- Public regression corpus: 18 MIT fixtures covering 14 extensions plus scanned/corrupt/embedded-image examples. Mac development checks passed 22 tests, formatting and Clippy with warnings denied.
- Independent review reproduced and corrected three concrete defects: OS hidden attributes were not excluded, plain directory rename could replace a concurrently created empty destination, and failed misnamed files reported the suffix rather than detected format. Added regressions; atomic NOREPLACE/RENAME_EXCL now protects publication on Unix, and native Windows uses directory move semantics.
- Independent forward execution found retained source-relative links with unresolved targets. Added a visible warning rather than expanding filesystem scope or claiming those targets were copied. Clarified that verify exit 0 confirms artifact integrity while partial/failed statuses remain.
- Plugin Eval: 95 initial, 91 development tree including build caches, 95 final clean source. Applied the trigger advice; retained useful fixture attribution despite the generic extra-document warning. Official Agent Skills validation passed. No scoring rules were altered.
- Corpus checks distinguish readable extraction from fidelity: PDFs showed differences against pypdf and PDFKit, so they remain partial. This is recorded rather than hidden as a successful lossless conversion.
