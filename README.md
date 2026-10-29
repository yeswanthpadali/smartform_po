# smartform_po
"smartform for purchase order invoice 
"driver program
REPORT ZSF1_PO.

parameters: p_iv type ebeln default '3000000034' ,
            p_rate type p default '5'.
data: gv_fnam type RS38L_FNAM.
data: ls_output_options type SSFCOMPOP.
data : ls_control_parameters type SSFCTRLOP.
DATA:    LS_JOB_OUTPUT_INFO TYPE   SSFCRESCL.
DATA: LT_LINES TYPE TABLE OF TLINE.

start-of-selection.
CALL FUNCTION 'SSF_FUNCTION_MODULE_NAME'
  EXPORTING
    FORMNAME                 = 'ZPO_INVOICE_1'
*   VARIANT                  = ' '
*   DIRECT_CALL              = ' '
  IMPORTING
    FM_NAME                  =  gv_fnam
* EXCEPTIONS
*   NO_FORM                  = 1
*   NO_FUNCTION_MODULE       = 2
*   OTHERS                   = 3
          .
IF SY-SUBRC <> 0.
* Implement suitable error handling here
ENDIF.

ls_control_parameters-NO_DIALOG = 'X'.
ls_control_parameters-PREVIEW = 'X'.
ls_OUTPUT_OPTIONS-TDDEST      = 'LP01'.
ls_control_parameters-GETOTF  = 'X'.

CALL FUNCTION gv_fnam
  EXPORTING
*   ARCHIVE_INDEX              =
*   ARCHIVE_INDEX_TAB          =
*   ARCHIVE_PARAMETERS         =
    CONTROL_PARAMETERS         = ls_control_parameters
*   MAIL_APPL_OBJ              =
*   MAIL_RECIPIENT             =
*   MAIL_SENDER                =
    OUTPUT_OPTIONS             =  ls_output_options
    USER_SETTINGS              = ' '
    IV_EBELN                   = p_iv
    IV_RATE                    = p_rate
  IMPORTING
*   DOCUMENT_OUTPUT_INFO       =
    JOB_OUTPUT_INFO            =  LS_JOB_OUTPUT_INFO
*   JOB_OUTPUT_OPTIONS         =
* EXCEPTIONS
*   FORMATTING_ERROR           = 1
*   INTERNAL_ERROR             = 2
*   SEND_ERROR                 = 3
*   USER_CANCELED              = 4
*   OTHERS                     = 5
          .
IF SY-SUBRC <> 0.
* Implement suitable error handling here
ENDIF.

CALL FUNCTION 'CONVERT_OTF'
 EXPORTING
   FORMAT                      = 'PDF'
*   MAX_LINEWIDTH               = 132
*   ARCHIVE_INDEX               = ' '
*   COPYNUMBER                  = 0
*   ASCII_BIDI_VIS2LOG          = ' '
*   PDF_DELETE_OTFTAB           = ' '
*   PDF_USERNAME                = ' '
*   PDF_PREVIEW                 = ' '
*   USE_CASCADING               = ' '
*   MODIFIED_PARAM_TABLE        =
* IMPORTING
*   BIN_FILESIZE                =
*   BIN_FILE                    =
  TABLES
    OTF                         = LS_JOB_OUTPUT_INFO-OTFDATA
    LINES                       = LT_LINES
* EXCEPTIONS
*   ERR_MAX_LINEWIDTH           = 1
*   ERR_FORMAT                  = 2
*   ERR_CONV_NOT_POSSIBLE       = 3
*   ERR_BAD_OTF                 = 4
*   OTHERS                      = 5
          .
IF SY-SUBRC <> 0.
* Implement suitable error handling here
ENDIF.


CALL FUNCTION 'GUI_DOWNLOAD'
  EXPORTING
*   BIN_FILESIZE                    =
    FILENAME                        = 'C:\Users\Dell\Downloads\ORDER.PDF'
    FILETYPE                        = 'BIN'
*   APPEND                          = ' '
*   WRITE_FIELD_SEPARATOR           = ' '
*   HEADER                          = '00'
*   TRUNC_TRAILING_BLANKS           = ' '
*   WRITE_LF                        = 'X'
*   COL_SELECT                      = ' '
*   COL_SELECT_MASK                 = ' '
*   DAT_MODE                        = ' '
*   CONFIRM_OVERWRITE               = ' '
*   NO_AUTH_CHECK                   = ' '
*   CODEPAGE                        = ' '
*   IGNORE_CERR                     = ABAP_TRUE
*   REPLACEMENT                     = '#'
*   WRITE_BOM                       = ' '
*   TRUNC_TRAILING_BLANKS_EOL       = 'X'
*   WK1_N_FORMAT                    = ' '
*   WK1_N_SIZE                      = ' '
*   WK1_T_FORMAT                    = ' '
*   WK1_T_SIZE                      = ' '
*   WRITE_LF_AFTER_LAST_LINE        = ABAP_TRUE
*   SHOW_TRANSFER_STATUS            = ABAP_TRUE
*   VIRUS_SCAN_PROFILE              = '/SCET/GUI_DOWNLOAD'
* IMPORTING
*   FILELENGTH                      =
  TABLES
    DATA_TAB                        = LT_LINES
*   FIELDNAMES                      =
* EXCEPTIONS
*   FILE_WRITE_ERROR                = 1
*   NO_BATCH                        = 2
*   GUI_REFUSE_FILETRANSFER         = 3
*   INVALID_TYPE                    = 4
*   NO_AUTHORITY                    = 5
*   UNKNOWN_ERROR                   = 6
*   HEADER_NOT_ALLOWED              = 7
*   SEPARATOR_NOT_ALLOWED           = 8
*   FILESIZE_NOT_ALLOWED            = 9
*   HEADER_TOO_LONG                 = 10
*   DP_ERROR_CREATE                 = 11
*   DP_ERROR_SEND                   = 12
*   DP_ERROR_WRITE                  = 13
*   UNKNOWN_DP_ERROR                = 14
*   ACCESS_DENIED                   = 15
*   DP_OUT_OF_MEMORY                = 16
*   DISK_FULL                       = 17
*   DP_TIMEOUT                      = 18
*   FILE_NOT_FOUND                  = 19
*   DATAPROVIDER_EXCEPTION          = 20
*   CONTROL_FLUSH_ERROR             = 21
*   OTHERS                          = 22
          .
IF SY-SUBRC <> 0.
* Implement suitable error handling here
ENDIF.
