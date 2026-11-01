# Validation record

Date: 2026-11-01. Results below distinguish static skill checks, deterministic Rust behavior, document extraction, and native platform execution.

## Static Skill evaluation

Plugin Eval initial static score: **95/100 (A)**. The official Agent Skills validator passed. The initial report recommended an explicit “Use when” trigger in the description; that recommendation was applied. Initial evaluation occurred before the Rust helper was complete, illustrating why its score cannot establish runtime behavior. Final clean-source evaluation: **95/100 (A)**, above the requested 90 threshold. The remaining warning flags a nested fixture README that is retained because it documents corpus licensing and correct test inputs. Development-tree evaluation was 91 because it included Cargo build artifacts. Both original reports and the source-only packaging condition are retained in [evaluation](evaluation/README.md).

The installed Plugin Eval only analyzes Python/TypeScript helpers. This project's Rust code is checked by Cargo tests and Clippy, not by that static code evaluator. Token figures in those reports are estimates, not measured agent usage.

## Public corpus

An expanded local upstream corpus contains 78 documents across 14 extensions: DOC, DOCX, PPT, PPTX, XLS, XLSX, XLSB, ODT, ODS, ODP, RTF, EPUB, CSV and PDF. Of these, 64 expected readable cases yielded readable Markdown, 12 malformed/resource-abuse cases failed in a controlled way, and 2 scanned-PDF cases returned needs_ocr. Preserved originals matched all 78 input hashes. Twenty extracted asset payloads (100,026 bytes) had no missing local asset links or mismatched ZIP payload bytes.

Sixty readable cases had upstream Markdown snapshots: 59 had complete normalized line coverage. The PDF case differed in a heading/line grouping and bidirectional-script ordering; its independent character 4-gram recall was 97.1%. These are diagnostic comparisons, not a general semantic accuracy score. The lockfile resolves pdf-inspector 1.25.2, while the upstream release's historical lockfile used 1.14.2; both satisfy anydoc 0.2.4's declared dependency range.

The committed redistributable subset contains 18 attributed MIT fixtures covering the 14 extensions, embedded assets, scanned PDF and a truncated PDF. Rust integration tests exercise these fixtures natively.

## Private real documents

The user authorized local Downloads testing. Names, source paths, individual source hashes, extracted text and originals are not committed. A separate, deduplicated corpus contains 97 documents from files and ZIP members (DOCX, CSV, PDF, EPUB); 95 generated readable Markdown (93 partial, 2 converted), while 2 truncated/corrupt PDFs failed in a controlled way. All 97 retained originals matched their input bytes, 271 extracted assets matched their source payloads, and 291 relative links resolved. The 82 original direct Downloads files were rehashed with no changes.

Independent checks retained all CSV cell values and all DOCX alphanumeric character multisets in this corpus. EPUB character-multiset recall ranged from 99.49% to 100%. For 23 readable PDFs, 10 had recall below 99% against independent extractors: minimum 89.0% versus pypdf, 91.6% versus PDFKit, with median 99.93%. These lexical differences require review and do not establish visual or semantic significance. PDF outputs remain partial. See [machine-readable aggregate evidence](corpus-validation.json).

## Native platform evidence

The GitHub Actions workflow runs formatting, Clippy, tests and a release build on macOS ARM64, macOS Intel and Windows. Its live run, final commit and artifact links are recorded after execution. A workflow definition alone is not a passed platform test.

## Limits

Structural verification checks recorded artifact bytes and safe paths. It does not prove arbitrary source fidelity, validate all Markdown syntax, reproduce original page geometry, perform OCR, or independently confirm facts. General format-wide warnings conservatively produce partial status even when text extraction succeeds. Known parser limitations remain visible rather than being hidden to improve a score.
