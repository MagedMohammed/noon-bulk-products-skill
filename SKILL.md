---
name: noon-bulk-products
description: Create, validate, and repair Noon Egypt bulk product, NIS catalog, offer, price, stock, and image spreadsheets. Use when converting WooCommerce or Jumia product data to Noon templates, mapping Noon categories, correcting rejected rows, or preparing only failed products for re-upload without duplicates.
---

# Noon Bulk Products

Prepare Noon Egypt bulk-upload files without changing the template structure.

## Required inputs

- Use the latest Noon template or export supplied by the user.
- Use the source catalog, product URLs, prices, stock, images, and category evidence supplied by the user.
- Read the template instructions, dropdown values, required fields, and category-specific attributes before editing.
- Preserve sheet names, hidden sheets, headers, formulas, validation lists, styles, and column order.

## Core workflow

1. Identify whether the file is a catalog/NIS upload, offer-price-stock upload, image repair, or an error report.
2. Match products by seller SKU. Never use row position as the primary key.
3. Map every product to the narrowest valid Noon leaf category. Use `references/noon-categories.md` for the known Kayan Store mappings, then confirm against the supplied Noon catalog/template.
4. Write English and Arabic content in their designated fields. Keep product identity, measurements, materials, compatibility, pack quantity, and color consistent across languages.
5. Use unique seller SKUs for separate products or color variants. Do not silently reuse the SKU of an already-live product.
6. Validate required fields and exact select-list values before export.
7. Save as `.xlsx` for catalog/NIS templates unless Noon explicitly requests CSV for the selected import flow.
8. Reopen the finished file and verify values, sheet structure, formulas, image URLs, and row count.

## Exact Noon Egypt VAT rule

- Set `vat_rate_eg` to exactly `Std` for every product row that requires the field.
- Treat this value as case-sensitive.
- Never use `std`, `STD`, `standard`, `Standard`, or `ستاندرد`.
- Before delivery, assert that every non-empty product row has `vat_rate_eg == "Std"`.
- If Noon reports `Invalid value '<value>' for select attribute 'vat_rate_eg'`, replace the rejected rows with `Std` and revalidate.

## Error-file repair

- Read the Noon error report and isolate only rejected or failed seller SKUs.
- Remove successful rows from the repair file so live products are not duplicated.
- Correct only the fields implicated by the error unless another deterministic validation failure is present.
- If products such as shadow lamps or mobile accessories are already live, exclude them unless the user explicitly requests an update operation.
- Keep a one-row-per-SKU repair list and report the number of corrected rows.

## Images

- Use direct, public HTTPS image URLs that return image bytes without authentication or redirects to an HTML page.
- Prefer JPG/JPEG with a clean product-focused composition and a marketplace-safe background.
- Confirm that each image URL is reachable and corresponds to the correct SKU and color.
- Do not change image URLs when preparing only a price or stock update.

## Offers, prices, and stock

- Keep seller SKU unchanged.
- Use the user's approved price formula. Do not infer a new markup from an older file.
- When instructed to calculate from Kayan Store prices, use the most recently approved formula rather than the historical 50% markup.
- Preserve current stock when the user asks to add only colors or images.
- Use the exact Noon field names and accepted enum values from the supplied template.

## Validation checklist

- No duplicate seller SKU exists in the outgoing operation.
- Only intended products are present.
- Successful products are absent from retry files.
- `vat_rate_eg` is exactly `Std`.
- Category and required category attributes are populated.
- Brand uses the user-approved value; use `Generic` only when instructed or appropriate.
- English and Arabic titles/descriptions identify the same item.
- Prices are numeric, non-negative, and follow the latest approved formula.
- Stock is numeric and matches the requested source.
- Image URLs are direct, public, and mapped to the right product/color.
- Workbook sheets, headers, dropdowns, and formatting remain intact.

## Deliverable

Return a ready-to-upload file and a concise summary containing total rows, excluded successful rows, applied fixes, and any unresolved fields that require the user's decision.
