#<script>
  $('.fq').after('<a class="btn btn-warning col-sm-6" style="float:left;" 
				 onclick="OpenPopupMultiBS(\'InsuranceDtl\',\'Item List\',\'ViewInsuranceDtl\',
				 'InsuredName,InsuredAddress,ContactNumber,SubPolicyNo,InstallmentDate,PremiumAmt\');">
				 Select Insurance'</a>');
</script>
<script>
  function GetInsuranceMaxSeqNo() {
    const refid = Number(
        $('#txtinsurancemstinrefranceid').val()
    );
    if (!refid) {
        console.log("Reference ID not found");
        insuranceSeqNo = 0;
        return;
    }
    var res = runScript(`USP_InsuranceDetails_MaxSeqNo ${refid}`);
    if (res && res.length > 0) {
      insuranceSeqNo = parseInt(res) || 0;
    } else {
        insuranceSeqNo = 0;
    }
}
function UploadData() {debugger;
    // =========================================
    // FIRST GET MAX SEQ FROM DATABASE
    // =========================================
    GetInsuranceMaxSeqNo();
    console.log(
        "MAX SEQ FROM DB =",
        insuranceSeqNo
    );

    // =========================================
    // GET UPLOAD DATA
    // =========================================

    var data = $('.fetchdata').val();
    if (!data || !data.trim()) {
        return;
    }
    var rows = data
        .replace(/\r?\n/g, ';')
        .split(';');

    // =========================================
    // PROCESS EACH ROW
    // =========================================
    for (var i = 0; i < rows.length; i++) {

        if (!rows[i].trim()) {
            continue;
        }
        // =====================================
        // GET BLANK ROW
        // =====================================

        var rowIndex =
            GetBlankInsuranceRow();
        // =====================================
        // IF NO BLANK ROW -> ADD NEW ROW
        // =====================================
        if (rowIndex === -1) {

            GetDevEditGridAddRow(
                'tblInsuranceDtlIN',
                'InsuranceDtlIN'
            );
            rowIndex =
                $('#tblInsuranceDtlIN tbody tr').length - 1;
        }
        // =====================================
        // INCREMENT SEQUENCE
        // =====================================

        insuranceSeqNo++;
        // =====================================
        // SET SEQ NO
        // =====================================

        $('#txtinsurancedtlinseqno' + rowIndex).val(insuranceSeqNo);
        // =====================================
        // SPLIT COLUMNS
        // =====================================

        var cols =rows[i].replace(/\t/g, ',').split(',');

        // =====================================
        // SET DATA
        // =====================================
        if (cols[0] !== undefined)$('#txtinsurancedtlininsuredname' + rowIndex).val(cols[0].trim());
        if (cols[1] !== undefined)$('#txtinsurancedtlininsuredaddress' + rowIndex).val(cols[1].trim());
        if (cols[2] !== undefined)$('#txtinsurancedtlincontactnumber' + rowIndex).val(cols[2].trim());
        if (cols[3] !== undefined)$('#txtinsurancedtlinsubpolicyno' + rowIndex).val(cols[3].trim());
        if (cols[4] !== undefined)$('#txtinsurancedtlininstno' + rowIndex).val(cols[4].trim());
        if (cols[5] !== undefined)$('#txtinsurancedtlininstallment' + rowIndex).val(cols[5].trim());
        if (cols[6] !== undefined)$('#txtinsurancedtlininstallmentdate' + rowIndex).val(cols[6].trim());
        if (cols[7] !== undefined)$('#txtinsurancedtlinpremiumamt' + rowIndex).val(cols[7].trim());
        if (cols[8] !== undefined)$('#txtinsurancedtlintaxamt' + rowIndex).val(cols[8].trim());
        // =====================================
        // AMOUNT = PREMIUM + TAX
        // =====================================
        var premium = parseFloat( $('#txtinsurancedtlinpremiumamt' + rowIndex).val()) || 0;
        var tax = parseFloat($('#txtinsurancedtlintaxamt' + rowIndex).val()) || 0;
        $('#txtinsurancedtlinamount' + rowIndex).val(premium + tax);
        if (cols[10] !== undefined)$('#txtinsurancedtlinpaiddate' + rowIndex).val(cols[10].trim());
        if (cols[11] !== undefined)$('#txtinsurancedtlininstallmentdtl' + rowIndex).val(cols[11].trim());
        if (cols[12] !== undefined)$('#txtinsurancedtlinpaiddetails' + rowIndex).val(cols[12].trim());
        if (cols[13] !== undefined)$('#txtinsurancedtlinpolicyexcdtl' + rowIndex).val(cols[13].trim());
        if (cols[14] !== undefined)$('#txtinsurancedtlinsystemdetails' + rowIndex).val(cols[14].trim());
        if (cols[15] !== undefined)$('#txtinsurancedtlinexcamountone' + rowIndex).val(cols[15].trim());
        if (cols[16] !== undefined)$('#txtinsurancedtlinexctypeone' + rowIndex).val(cols[16].trim());
        if (cols[17] !== undefined)$('#txtinsurancedtlinexcamounttwo' + rowIndex).val(cols[17].trim());
        if (cols[18] !== undefined)$('#txtinsurancedtlinexctypetwo' + rowIndex).val(cols[18].trim());
        if (cols[19] !== undefined)$('#txtinsurancedtlinexcamountthree' + rowIndex).val(cols[19].trim());
        if (cols[20] !== undefined)$('#txtinsurancedtlinexctypethree' + rowIndex).val(cols[20].trim());
    }
}

function GetBlankInsuranceRow() {
    var blankRow = -1;
    $('[id^="txtinsurancedtlininsuredname"]').each(function () {
        var id = $(this).attr('id');
        var rowIndex = id.replace('txtinsurancedtlininsuredname', '');
        var name = $(this).val();
        var address = $('#txtinsurancedtlininsuredaddress' + rowIndex).val();
        var contact = $('#txtinsurancedtlincontactnumber' + rowIndex).val();
        if (!name && !address && !contact) {
            blankRow = rowIndex;
            return false;
        }
    });

    return blankRow;
}  
InsCalc();  
</script>
 
