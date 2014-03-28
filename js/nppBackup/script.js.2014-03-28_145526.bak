var ngSO = angular.module('ngSO', []);
ngSO.controller("SOListController", ['$scope', 'SOFactory', '$timeout', function ($scope, SOFactory, $timeout) {
    $scope.getAnswers= function(user){
        SOFactory.fetch('https://api.stackexchange.com/2.2/users/'+user+'/answers?order=desc&sort=activity&site=stackoverflow').then(function (response) {        
            var answers=response.data.items.splice(0,15);
            var questions=[];
            angular.forEach(answers, function(answer){
                questions.push(answer.question_id);
                answer.timeago=$.timeago(new Date(answer.creation_date * 1000));
            });            
            SOFactory.fetch('https://api.stackexchange.com/2.2/questions/'+questions.join(";")+'?site=stackoverflow').then(function (response) {        
                angular.forEach(response.data.items, function(q, i){
                    var a = _.find( answers, function( o ) {
                        if(o.question_id === q.question_id){return o;}
                    });
                    a.parentQuestion=q;
                });                     
            });
            $timeout(function(){
                $scope.answers=answers;
            });                
        });
    }    
}]);
ngSO.factory('SOFactory', ['$http', '$timeout', function ($http, $timeout) {
    var factory = {
        fetch : function (url) {
            return $http.jsonp(url+'&callback=JSON_CALLBACK').success(function (data) {
                return data;
            }).error(function (e) {
                return e;
            });
        }
    };
    return factory;    
}]);