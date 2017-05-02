# about-pht-Card-voucher-Set-up-the-router

通过文件夹中创建index.html 与 index.js来做为vue的模板与js方法

var routes = new VueRouter({
    mode: 'hash',
    base: './',
    linkActiveClass: 'active',
    routes: [
      {
        path: '/:module/:action?',
      },
      {
        path: '/pages/:action?',
      },
    ],
  });

  requirejs.config({
    urlArgs: "t=" +  (new Date()).getTime(),
  });
  routes.beforeEach((to, from, next) => {
    
    var module = to.params.module || 'Kit';
    var action = to.params.action || 'index';

    axios.get('./pages/' + module + '/' + action + '.html').then(res => {
      require(['./pages/' + module + '/' + action], data => {

        Vue.component(module + '-' + action, Object.assign({
          template: res.data,

        }, data));

        vm.currentView = module + '-' + action;

        next();
      });
    });
  });
  
  var vm = new Vue({
    el: '#wrapper',
    router: routes,
    data() {
      return {
          currentView: '',
          PageLoad: false,
          year: 0,
          month: 0,
          day: 0,
          time: 0,
      };
    },
  });
