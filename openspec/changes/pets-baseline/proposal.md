# Proposal

## Why

PC-301 will change the pet add and edit forms. A change can only be written as a delta, and reviewed, against a spec of what the forms do today. No such spec exists.

## What Changes

- Record the current behaviour of the pet add and edit forms as a new capability, from the 29 characterization tests in `PetControllerCharacterizationTests`.
- No code or tests change.

### The sort

**Requirements (24):** createSavesOwnerWithNewPetAndFlashesMessage, editUpdatesExistingPetAndFlashesMessage, editCopiesNameBirthDateAndTypeOntoOwnersPetWhenInstancesDiffer, createBlankNameReportsOnlyRequiredNotDuplicate, editBlankNameReportsOnlyRequiredNotDuplicate, createNameOf30CharsAccepted, createNameOf31CharsRejectedWithSize, editNameOf31CharsRejectedWithSize, createDuplicateNameIsCaseInsensitive, editDuplicateNameOfOtherPetIsCaseInsensitive, editOwnNameInDifferentCaseIsAllowed, createUniqueViolationMessageMatchedCaseInsensitively, createOtherIntegrityViolationIsRethrown, editUniqueViolationMessageMatchedCaseInsensitively, editOtherIntegrityViolationIsRethrown, createMissingBirthDateIsRequired, editMissingBirthDateIsRequiredAndNotSaved, createAcceptsBirthDateOfToday, createUnparseableBirthDateReportsTypeMismatchAndRequired, editAcceptsBirthDateOfToday, createReportsEveryValidatorErrorAtOnce, createReportsDuplicateNameAndFutureBirthDateTogether, createIgnoresSubmittedId, editIgnoresSubmittedId.

**Incidental (2), not specified:**
- createDuplicateCheckIgnoresUnsavedPetsOfOwner: needs an unsaved pet in the owner's list during a POST; the owner is loaded fresh for each request.
- editDoesNotRequireType: the edit form always sends a type, and `pets.type_id` is NOT NULL.

**Suspects (3), open questions for a person:**
- Q1 createRejectsBirthDateOfTomorrowWithCodeOnlyAndDoesNotSave: a future birth date gets "invalid date", the message for a malformed date. Specify as current behaviour, or leave out?
- Q2 editRejectedFormHasAlreadyBoundValuesOntoOwnersPet: a rejected edit has already bound the submitted values onto the loaded pet. Specify, or leave out?
- Q3 editUnknownPetIdFailsWithNullPointerAndDoesNotSave: an unknown pet id ends in a NullPointerException and the error page. Specify, or leave out?

## Capabilities

### New Capabilities
- `pets`: adding and editing a pet through the owner's pet forms

### Modified Capabilities

## Impact

None on code. PC-301's change will be written as a delta against `pets`.
