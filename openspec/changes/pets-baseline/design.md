# Design

## Context

No code changes. This records which tests each requirement comes from.

## Evidence per requirement


Moved out of the spec on 31 October, because `openspec validate --strict` warns when a requirement's text passes 500 characters.

| Requirement | Tests |
|---|---|
| Add a pet to an owner | run C `createSavesOwnerWithNewPetAndFlashesMessage`; run A `CreationForm.successAddsPetToOwnerSavesAndFlashesMessage`; run B `AddForm.successSavesOwnerWithNewPetAndFlashesMessage`; `PetControllerTests.processCreationFormSuccess`. All mocked. |
| Edit a pet in place | run C `editUpdatesExistingPetAndFlashesMessage`, `editCopiesNameBirthDateAndTypeOntoOwnersPetWhenInstancesDiffer`; run A `UpdateForm.successUpdatesExistingPetInPlaceSavesAndFlashesMessage`; run B `EditForm.successUpdatesExistingPetSavesAndFlashesMessage`; `PetControllerTests.processUpdateFormSuccess`. All mocked. |
| Pet name is required | run C `createBlankNameReportsOnlyRequiredNotDuplicate`, `editBlankNameReportsOnlyRequiredNotDuplicate`; run A `CreationForm.blankNameIsOnlyRequiredNotDuplicateChecked`; run B `AddForm.validationErrorDoesNotSave`, `EditForm.validationErrorDoesNotSave`; `PetControllerTests.processCreationFormWithBlankName`, `processUpdateFormWithBlankName`. All mocked. |
| Pet name is at most 30 characters | run C `createNameOf30CharsAccepted`, `createNameOf31CharsRejectedWithSize`, `editNameOf31CharsRejectedWithSize`; run A `CreationForm` and `UpdateForm`, `nameOfThirtyCharactersIsAccepted` and `nameOfThirtyOneCharactersIsRejectedWithSize`; run B `AddForm` and `EditForm`, the same boundary. All mocked. |
| Pet name is unique per owner, ignoring case | run C `createDuplicateNameIsCaseInsensitive`, `editDuplicateNameOfOtherPetIsCaseInsensitive`, `editOwnNameInDifferentCaseIsAllowed`; run A `CreationForm.duplicateNameCheckIgnoresCase`, `UpdateForm.duplicateNameCheckIgnoresCase`, `UpdateForm.renamingToOwnNameInDifferentCaseIsAccepted`; run B `AddForm.duplicateNameIsCaseInsensitive`, `EditForm.duplicateNameOfEarlierPetIsCaseInsensitive`, `EditForm.keepingOwnNameInOtherCaseIsAccepted`; `PetControllerTests.processCreationFormWithDuplicateName`, `processUpdateFormWithDuplicateName`, `processUpdateFormWithSameName`. All mocked. |
| Duplicate names caught by the database | runs A, B and C, `...UniqueViolationMessageMatched...` or `uniqueConstraintMessageIsMatched...` and `otherDataIntegrityViolationIsRethrown` on both forms, mocked; `PetClinicConcurrencyTests.testDuplicatePetNameRaceConditionIsBlocked`, the running application on H2, add form only. |
| Birth date is required | run C `createMissingBirthDateIsRequired`, `editMissingBirthDateIsRequiredAndNotSaved`; run A `CreationForm.missingBirthDateIsRejectedAsRequired`, `UpdateForm.missingBirthDateIsRejectedAsRequired`; run B `AddForm.emptyBirthDateIsRequired`, `EditForm.emptyBirthDateIsRequired`. All mocked. |
| Birth date is a valid date, not in the future | run C `createAcceptsBirthDateOfToday`, `createRejectsBirthDateOfTomorrowWithCodeOnlyAndDoesNotSave`, `createUnparseableBirthDateReportsTypeMismatchAndRequired`, `editAcceptsBirthDateOfToday`; run A `CreationForm.birthDateTodayIsAccepted`, `birthDateTomorrowIsRejectedWithTypeMismatchBirthDate`, `malformedBirthDateIsRejectedAsTypeMismatch`, `UpdateForm.birthDateTodayIsAccepted`; run B the same on both forms; `PetControllerTests.processCreationFormWithInvalidBirthDate`; `PetControllerTests.processUpdateFormRejectsFutureBirthDate`, written in class at Week 2 step 4 and green after step 7. All mocked. |
| Pet type is required when adding a pet | run C `createReportsEveryValidatorErrorAtOnce`; run A `CreationForm.allErrorsAreReportedTogetherInOrder`; run B `AddForm.idParameterIsIgnoredSoTypeIsStillRequired`, `AddForm.unknownTypeIsTypeMismatch`; `PetControllerTests.processCreationFormWithMissingPetType`. All mocked. |
| Validation errors are reported together | run C `createReportsDuplicateNameAndFutureBirthDateTogether`, `createReportsEveryValidatorErrorAtOnce`; run A `CreationForm.allErrorsAreReportedTogetherInOrder` and `UpdateForm.allErrorsAreReportedTogetherInOrder`, which Week 2 step 6 changed to check the errors as a set; run B `AddForm.allErrorsAreReportedTogether`, `EditForm.allErrorsAreReportedTogether`. All mocked. |
| The pet id cannot be set from the form | run C `createIgnoresSubmittedId`, `editIgnoresSubmittedId`; run A `UpdateForm.idParameterIsNotBound`; run B `AddForm.idParameterIsIgnoredSoTypeIsStillRequired`. All mocked. |

