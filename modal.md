# modaldoc
modal de documentos
<style>
    .fila {
        height: 45px;
        border-bottom: 1px solid rgba(9, 19, 46, .1);
    }

    .btcel {
        text-align: center;
    }

    .btload a {
        color: white;
        text-decoration: none;
        text-align: center;
    }

    .btload a:visited {
        color: white;
    }

    .ttitle {
        text-align: center;
        font-size: 18px;
        background-color: #E0E5F1;
        height: 40px;
        border-top-left-radius: 8px;
        border-top-right-radius: 8px;
    }

    .totales {
        height: 45px;
        font-size: 18px;
        padding-top: 12px;
    }

    #tsalida tbody {
        display: inline-block;
        height: 500px;
        /*overflow:auto;*/
        overflow-y: auto;
        overflow-x: hidden;
        border: 1px solid rgba(9, 19, 46, .1);
        width: 100%;
    }

    .corr {
        text-align: right;
    }

    .estadodoc {
        text-align: center;
    }

    .customFooter {
        border-top: 1px solid rgba(9, 19, 46, .1);
        background-color: @@estilos->colorfooter;
        /*#f4f7fd!important*/
        padding-left: 7px;
        height: 50px;
    }

    button[type='submit'] {
        margin: 6px 0 0 5px;
    }

    .material-icons {
        vertical-align: middle;
    }
</style>

<span class="titparr titparrJS">JAVASCRIPT</span>
<!-- <table id="tsalida" width="100%"></table> -->

<script>
    $(document).ready(function () {

        /*RESUMEN: MIVI F1*/

        /*este if permite ejecutar todo el codigo solo si
        se esta ejecutando el tramite y no en su diseño*/
        if ($("i:contains('edit')").length == 0) {

            ets = []; docname = [];

            ttit = ('@@t_titular' == 'a_persona_natural' ? 'n' : 'j');
            tdocprop = '@@tipo_documento1'; tdocprop = tdocprop.replaceAll('_', ' ').toUpperCase();;//p. natural
            tdocrepre = '@@tipo_documento_repre'; tdocrepre = tdocrepre.replaceAll('_', ' ').toUpperCase();

            tdocprop = (ttit == 'n' ? tdocprop : tdocrepre);

            ets[0] = '@@catreq->c1->etq'; //CREDENCIAL DEL REPRESENTANTE LEGAL *
            ets[1] = '@@catreq->c28->etq'; //Matrícula de comercio *
            ets[2] = '@@catreq->c6->etq'; //NIT de la sociedad *
            ets[3] = '@@catreq->c7->etq'; //Solvencia de la DGII de la sociedad *
            ets[4] = '@@catreq->c45->etq'; //Solvencia de la DGII de accionistas *
            ets[5] = '@@catreq->c9->etq'; //Documento de identidad  del solicitante (representante legal) *
            ets[6] = '@@catreq->c10->etq'; //Documento de identidad del solicitante apoderado *
            ets[7] = '@@catreq->c11->etq'; //Documento de identidad del solicitante de accionistas *
            ets[8] = '@@catreq->c46->etq'; //Plano que diferencie el área original y el área que se reduce o amplía *
            ets[9] = '@@catreq->c32->etq'; //NRC de la sociedad *
            ets[10] = '@@catreq->c47->etq'; //Plano del terreno donde se desarrollará el proyecto y su respectivo esquema de ubicación (anteproyecto de urbanización y edificio tipo) *
            ets[11] = '@@catreq->c48->etq'; //Copia del formulario ambiental emitido por el MARN *
            ets[12] = '@@catreq->c49->etq'; //Plano de conjunto que refleje la integración del terreno que será ampliado *
            ets[13] = '@@catreq->c50->etq'; //Resolución de calificación del lugar, línea de construcción y drenaje de aguas lluvias, emitidos por la entidad competente *
            ets[14] = '@@catreq->c51->etq'; //Dictamen de auditores externos que garanticen la inversión realizada *
            ets[15] = '@@catreq->c52->etq'; //Folder con planos individuales de las nuevas inversiones realizadas *
         


            docname[0] = '@@catreq->c1->var'; //CREDENCIAL DEL REPRESENTANTE LEGAL
            docname[1] = '@@catreq->c28->var'; //Matrícula de comercio
            docname[2] = '@@catreq->c6->var'; //NIT de la sociedad *
            docname[3] = '@@catreq->c7->var'; //Solvencia de la DGII de la sociedad *
            docname[4] = '@@catreq->c45->var'; //Solvencia de la DGII de accionistas *
            docname[5] = '@@catreq->c9->var'; //Documento de identidad  del solicitante (representante legal) *
            docname[6] = '@@catreq->c10->var'; //Documento de identidad del solicitante apoderado *
            docname[7] = '@@catreq->c11->var'; //Documento de identidad del solicitante de accionistas *
            docname[8] = '@@catreq->c46->var'; //Plano que diferencie el área original y el área que se reduce o amplía *
            docname[9] = '@@catreq->c32->var'; //NRC de la sociedad *
            docname[10] = '@@catreq->c47->var'; //Plano del terreno donde se desarrollará el proyecto y su respectivo esquema de ubicación (anteproyecto de urbanización y edificio tipo) *
            docname[11] = '@@catreq->c48->var'; //Copia del formulario ambiental emitido por el MARN *
            docname[12] = '@@catreq->c49->var'; //Plano de conjunto que refleje la integración del terreno que será ampliado *
            docname[13] = '@@catreq->c50->var'; //Resolución de calificación del lugar, línea de construcción y drenaje de aguas lluvias, emitidos por la entidad competente *
            docname[14] = '@@catreq->c51->var'; //Dictamen de auditores externos que garanticen la inversión realizada *
            docname[15] = '@@catreq->c52->var'; //Folder con planos individuales de las nuevas inversiones realizadas *
        
            url = window.location.origin;
            // btclass = "btview"; bttext = '<span class="viewtx">Visualizar</span>';
            // link1 = ''; link2 = '';
            rows = '<thead><tr><th colspan="3" class="ttitle"><b>DOCUMENTOS</b></th></tr></thead><tbody>';

            //crea resumen
            subidos = 0; pend = 0; corr = 0;
            //let ccastral = 12;
            for (i = 0; i < docname.length; i++) {
                corr++;
                ext = docname[i].substring(docname[i].length - 3);
                //link1 = '<a href="' + docname[i] + '">'; link2 = '</a>';
                link1 = ''; link2 = '';
                btclass = "btview"; bttext = '<span class="viewtx">Visualizar</span>';

                subidos++;
                rows += '<tr class="fila" width="100%"><td class="corr col01">' + corr + ')&nbsp</td><td class="col02" width="75%">' + ets[i] + '</td><td width="20%" class="btcel col03">' + link1 + '<button type="button" class="' + btclass + ' btn btn-secondary" data-nm="' + docname[i] + '" data-corr="' + corr + '">' + bttext + '</button>' + link2 + '</td></tr>';
            }
            //rows += "</tbody><tfoot><tr class='totales'><td colspan='3'>Subidos: <b>" + subidos + "</b> de <b>" + corr + "</b> | Pendientes: <b><span style='color:red'>" + pend + "</span></b></td></tr></tfoot>";
            //agrega evento click a cada boton
            $(document).on('click', 'button.btview', function () {

                ruta = url + '/uploads/datos/';

                doc = ruta + $(this).attr("data-nm");

                $('#ifr1').attr('src', doc);
                $('#modal1').modal('show');
            });

            $("#tsalida").html(rows);
            // tbw = parseInt($("#tsalida tbody").css("width"));
            // $(".col01").css("width", (tbw * 0.05));
            // $(".col02").css("width", (tbw * 0.75));
            // $(".col03").css("width", (tbw * 0.20));

            let maxrows = 15;
            let altofila = parseInt($("#tsalida tbody tr").css("height"));
            if (corr <= maxrows) {
                altotabla = ((altofila * corr) + 3) + "px";
            } else {
                altotabla = (altofila * maxrows) + "px";
            }
            $("#tsalida tbody").css("height", altotabla);

            /*customizacion de footer */
            $(".form-actions").addClass("customFooter");
            $("a").each(function () {
                if ($(this).html().trim() == 'Volver') {
                    $(this).css("margin", "6px 0 0 5px");
                }
            });
            /*arreglar bootones subir desde perfil*/
            $(".qq-upload-butto").each(function () {
                $(this).addClass("qq-upload-button");
            });
            /*identificadores de parrafos*/
            $('.titparr').each(function () {
                if ($(this).html() != "HR") {
                    $(this).parents('div.docname').removeClass('col-md-12');
                }
                $(this).hide();
            });

        } else {
            //agrega onlclick a parrafos
            $('.titparr').each(function () {
                idcampo = $(this).parents('div.campo').attr('data-id');
                $(this).attr('onclick', 'return editarCampo(' + idcampo + ')');
                $(this).addClass('btn btn-warning');
            });
            $('.titparrJS').removeClass('btn-warning').addClass('btn-dark');
        }    //IF RUNNING
    });
</script>
<span class="titparr">MODAL DOCS</span>
<!--MODAL 1-->
<div class="modal fade bd-example-modal-xl" tabindex="-1" role="dialog" aria-labelledby="myExtraLargeModalLabel"
    aria-hidden="true" id="modal1">
    <div class="modal-dialog modal-xl">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Documento adjuntado</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <iframe src="" id="ifr1" style="width: 100%;height: 500px;"></iframe>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>

            </div>
        </div>
    </div>
</div>
