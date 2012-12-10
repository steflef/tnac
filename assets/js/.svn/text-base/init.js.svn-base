function loadData(circons, hover, elem) {
    if (hover)
        loadHover(circons, elem);
    else {
        //createDelegMap(circons, elem);
        loadAggResult(circons);
    }
}
/////////////////////////////////////
///  call service  to get elected ( elus : french ) assembly  memebers 
/// params : circons : circonscription id  / elem : div id ( or else ) this param is mainly used to  get the hovered zone , we dont use it for now ( the easy way )
//////////////////////////////////
function loadHover(circons, elem) {

    $.getJSON('http://opendatatn.org/elus/' + circons + '?callback=?')
      .success(function (data) { buildHoverView(data, elem); })
      .error(function () { alert("error"); });
   

}
//////////////////////////////////////////////////////////////
function loadDelegationData(circons, deleg, hover, elem) {
    if (hover)
        loadDelegHover(circons, deleg, elem);
    else
        loadAggResult(circons + "/" + deleg);
}
////////////////////////////////////////////////////////
function loadDelegHover(circons, deleg, elem) {
  
    $.getJSON('http://opendatatn.org/agg' + "/" + circons + "/" + deleg + '?callback=?')
      .success(function (data) { buildDelegHoverView(data, elem); })
      .error(function () { alert("error"); });
}
//////////////////////////////////////////////////////
function buildDelegHoverView(data, elem) {
    header = data.delegation.name;
    html = "<ul>";
    html = html + "<li> votes:" + data.resultat.electeurs.votant + "</li>";
    html = html + "<li> enregistres:" + data.resultat.electeurs.enregistre + "</li>";
    html = html + "<li> votes blancs:" + data.resultat.bulletins.blancs + "</li>";
    html = html + "<li> votes annules:" + data.resultat.bulletins.annule + "</li>";
    html = html + "<li> bulletins delivres:" + data.resultat.bulletins.delivre + "</li>";
    html = html + "<li> bulletins endommage:" + data.resultat.bulletins.endommage + "</li></ul>";
    $("#summaryZone").html("<strong>"+header+"</strong"+html);

}
///////////////////////////////////////////////////////////////


function createDelegMap(circons, elem) {
   // var surl = 'jsonp.php?circ=' + circons+'?callback=?';

   // $.getJSON(surl, function (data) {
    $.getJSON('svg/' + circons + '.json', function (data) {
        var matrix = data.matrix;
        var f = elem.getBBox(false);
        var paths = data;
        $("#map").html("");
        var r = Raphael('map', 1200, 820),
		attributes = {
		    fill: '#fff',
		    stroke: '#3899E6',
		    'stroke-width': 1,
		    'stroke-linejoin': 'round'
		},
		arr = new Array();
        for (var deleg in paths) {
            if (deleg == 'matrix') {
                continue;
            }
            var obj = r.path(paths[deleg].path);
            obj.attr(attributes);
            arr[obj.id] = deleg;

            obj.hover(function () {
                var s = paths[arr[this.id]].deleg;
                loadDelegationData(circons, s, true, this);
                this.animate({
                    fill: randomColor(),
                    stroke: '#9090ff',
                    'stroke-width': 2
                }, 300);

            }, function () {
                this.animate({
                    fill: attributes.fill,
                    stroke: attributes.stroke,
                    'stroke-width': 1
                }, 300);
            }).click(function () {
                document.location.hash = arr[this.id];

                var s = paths[arr[this.id]].deleg;
                loadDelegationData(circons, s, false, this);
                $("#tabstrip").show();
            });

        }

    });

}
/////////////////////
function buildHoverView(data, elem) {

    header = "<span class='label label-warning'>" +data.circonscription.name+ "</span>" +"&nbsp;&nbsp;Sieges:<code>" + data.circonscription.nb_sieges+"</code><br/>";   // 
    html = "";
    
    for (i in data.listes) {
        l = data.listes[i];
        s = "<span class='label label-important'>" + l.elus.length + "</span>&nbsp;&nbsp;" + l.name
				+ "&nbsp;&nbsp;<small>P</small>&nbsp;<code>" + l.pourentage.toFixed(2)
				+ "%</code>&nbsp;&nbsp;";
        html = html + s;
    }
      
       $("#summaryZone").html(header+html);

}
//////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////
function getSummary(obj) {
     $.getJSON('http://opendatatn.org/agg' + "/" + obj + '?callback=?')
      .success(function (data) {
          var circ_name = data.circonscription.name;
          var total_votes = data.resultat.electeurs.votant;
          var total_enregistre = data.resultat.electeurs.enregistre;
          var correct = data.resultat.bulletins.correct;
          var delivres = data.resultat.bulletins.delivre;
          $("#cname").html(circ_name);
           var reg= "<div class='bar' style='width: 71%;'>"+total_enregistre+"</div>";
          $("#registred").html(reg);
          var total=parseInt(total_enregistre)*100/71;
          var del= parseInt(delivres)/total*100;
          var delProgress= "<div class='bar' style='width:"+del+"%;'>"+delivres+"</div>";
          $("#extraits").html(delProgress);
           var vald= parseInt(correct )/total*100;
          var delProgress= "<div class='bar' style='width:"+vald+"%;'>"+correct +"</div>";
          $("#valides").html(delProgress);
          
         
          
          
      })
      .error(function () { alert("error"); });

}
/////////////////////////////////////////////////////
////////////////////////////////////////////////////
function randomColor(){
return  '#'+Math.floor(Math.random()*16777216).toString(16);
}


//////////////////////////

function showMap(zoneid) {
  if (zoneid=='paths')
  {
     $('#breadcrump').hide('slow');


 }
    $("#map").html("");
    var zone = eval(zoneid);
   
    var r = Raphael('map', 1200, 820),
		attributes = {
		    fill: '#fff',
		    stroke: '#3899E6',
		    'stroke-width': 1,
		    'stroke-linejoin': 'round'
		},
		arr = new Array();

		for (var country in zone) {
		    var obj = r.path(zone[country].path);

        obj.attr(attributes);

        arr[obj.id] = country;

        obj
		.hover(function () {
		    this.animate({
		        fill: randomColor(),
		        stroke: '#9090ff',
		        'stroke-width': 2
		    }, 30);
		    var s = paths[arr[this.id]].circons;
		  

    						
		    loadData(s, true, this);
		   $("#summaryZone").show();
		}, function () {

		    this.animate({
		        fill: attributes.fill,
		        stroke: attributes.stroke,
		        'stroke-width': 1
		    }, 30);
		  $("#summaryZone").hide();
		})
		.click(function () {
		    var s = paths[arr[this.id]].circons;
		    loadData(s, false, this);
             
		    getSummary(s);
            $("#tabstrip").show();
		   
		});

		obj.dblclick(function () {
		    var s = paths[arr[this.id]].circons;


		    $('#breadcrump').show('slow');
		    // processResults('151');
		    createDelegMap(s, this);
		   $("#tabstrip").hide();
		});


    }


}


// function and all reate windows 
// this helps to create many window views

function createWindow(target, sender,data) {
 
   var window = $(target),
                   undo = $(sender)
                    .bind("click", function () {
                        console.log(window);
                        window.data(data).open();

                    });

    var onClose = function () {
        //  undo.show();

    }
    var onDeactivate=function(){
      
    }

    if (! window.data(data)) {
        window.kendoWindow({
            width: "360px",
            title: data,
            close: onClose,
            deactivate: onDeactivate
        });
    }






}

showMap('paths');