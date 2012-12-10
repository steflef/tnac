
//Not used 
function getPieData(listes,totalVote){
	var container = new Array();
	var p=0;
	var cumul=0;
	for (i in listes){
		var obj=listes[i];
		var newObj=new Array();
		newObj[0]=obj.name;
		newObj[1]=obj.vote;
		cumul+=obj.vote;
		container[i]=newObj;
		p=p+obj.pourcentage;
		if (obj.pourcentage<2 || (p>=90 && p<=99)){
			var lastObj=new Array();
			var r=listes.length-parseInt(i)-1;
			lastObj[0]="Autres ("+r+" listes)";
			lastObj[1]=totalVote-cumul;
			container[parseInt(i)+1]=lastObj;
			break;
		}
		
	}
	return container;
}
function getKendoPieData(listes, totalVote) {
    var container = new Array();
    var p = 0;
    var cumul = 0;
    for (i in listes) {
        var obj = listes[i];
        var newObj = new Object();
        newObj.category = obj.name;
        newObj.value = obj.vote;
        cumul += obj.vote;
        container[i] = newObj;
        p = p + obj.pourcentage;
        if (obj.pourcentage < 2 || (p >= 90 && p <= 99)) {
            var lastObj = new Object();
            var r = listes.length - parseInt(i) - 1;
            lastObj.category = "Autres (" + r + " listes)";
            lastObj.value = totalVote - cumul;
            container[parseInt(i) + 1] = lastObj;
            break;
        }

    }
    return container;
}

function createChart(data) {
    $("#chart").kendoChart({
                    title: {
              text: "Resultat par Bureau de vote"
          },
          legend: {
              visible:true,
              position: "bottom"
          },
          seriesDefaults: {
              labels: {
                  visible: false,
                  template: "${ category } #= kendo.format('{0:P}', percentage)#"
              }
          },
          series: [{
              type: "pie",
              data: getKendoPieData(data.resultat.listes,data.resultat.bulletins.correct)
          		}],
             tooltip: {
              visible: true,
              template: "${ category } #= kendo.format('{0:P}', percentage)#"
               },
          seriesClick: onSeriesClick
            
      });
  }
  function createGrid(data) {
      $("#grid").html("");
      $("#grid").kendoGrid({
          dataSource: {
              data: data.resultat.listes,
              pageSize: 10
          },
          groupable: true,
          scrollable: true,
          sortable: true,
          pageable: true,
          columns: [{
              field: "num",
              width: 30,
              title: "num"
          }, {
              field: "name",
              width: 120,
              title: "Nom Liste"
          }, {
              width: 40,
              field: "vote"
          }, {
              width: 120,
              field: "pourcentage"
          }
                        ]
      });
  
  
  }
 function onSeriesClick(e)
  {

     $.msg(kendo.format("{0} :{1}% <br/>data1 <br/> data2 <br/>",  e.value),{ header: e.category });
  }

function createbars(voters) {
  $("#bars").kendoChart({
                        theme: $(document).data("kendoSkin") || "default",
                        dataSource: {
                            data: voters
                        },
                        title: {
                            text: "Details bulletins"
                        },
                        legend: {
                            position: "bottom"
                        },
                        seriesDefaults: {
                            type: "bar",
                            labels: {
                                visible: true,
                                format: "{0}%"
                            }
                        },
                        series: [{
                            field: "value",
                            name: voters.circ
                        }],
                        valueAxis: {
                            labels: {
                                format: "{0}"
                            }
                        },
                        categoryAxis: {
                            field: "cat"
                        }
                    });
                }
            
function processResults(obj) {

 $.getJSON('http://opendatatn.org/agg/' + obj + '?callback=?')
      .success(function (data) { buildView(data); })
      .error(function () { alert("error"); });
}
function loadAggResult(path) {
  
 $.getJSON('http://opendatatn.org/agg/' + path + '?callback=?')
      .success(function (data) { buildView(data); })
      .error(function () { alert("error"); });
}


function buildView(data) 
{
    if (data.error == 0) {
        $("#putputpie").html("NO DATA AVAILABLE FROM ISIE");
        return;
    }
    var circ_name=data.circonscription.name;
    var total_votes= data.resultat.electeurs.votant;
    var total_enregistre =data.resultat.electeurs.enregistre;
    var correct=data.resultat.bulletins.correct;
    var delivres=data.resultat.bulletins.delivre;
    var circ_id= 151;
   
    var s= circ_name + "<div class='chart-wrapper'><div id='chart'></div></div>";
    var   voters = [ {
                        "circ": circ_name ,
                        "cat": "Total votes",
                        "value": total_votes
                    }, {
                        "circ": circ_name ,
                        "cat": "Total enregistes",
                        "value": total_enregistre
                    }, {
                        "circ": circ_name ,
                        "cat": "Belletins corrects",
                        "value": correct
                    },  {
                       "circ": circ_name ,
                        "cat": "Belletins delivres",
                        "value": delivres
                    } ];
      $("#putputpie").html(s);
       var ss= circ_name + "<div class='chart-wrappe1r'><div id='bars'></div></div>";
        $("#outputbars").html(ss);



        setTimeout(function () {
            createChart(data);
            createGrid(data);
            // Initialize the chart with a delay to make sure
            // the initial animation is visible
        }, 400)           
}