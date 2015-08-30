# aqua-currentuser
A component plugin that helps display the current user for the [Aqua](https://github.com/jedireza/aqua) website and user system (Hapi/React/Flux).

## Example 
Using /client/pages/account/components/NavBar.jsx...

```javascript
var React = require('react/addons');
var ReactRouter = require('react-router');
var ClassNames = require('classnames');
var Actions = require('../../../../client/components/currentUser/Actions');
var UserStore = require('../../../../client/components/currentUser/stores/User');


var Link = ReactRouter.Link;


var Component = React.createClass({
    contextTypes: {
        router: React.PropTypes.func
    },
    getInitialState: function () {

        Actions.getUserSettings();
        
        return {
             navBarOpen: false
         },
         this.getStateFromStores();

    },
    isNavActive: function (routes) {

        return ClassNames({
            active: routes.some(function (route) {

                return this.context.router.isActive(route);
            }.bind(this))
        });
    },
    toggleMenu: function () {

        this.setState({ navBarOpen: !this.state.navBarOpen });
    },
    componentWillReceiveProps: function () {

        this.setState({ 
            navBarOpen: false
        });
    },
    getStateFromStores: function () {

        return {
            user: UserStore.getState()
        };
    },
    componentDidMount: function () {

        UserStore.addChangeListener(this.onStoreChange);
    },
    onStoreChange: function () {

        this.setState(this.getStateFromStores());
    },
    render: function () {

        var navBarCollapse = ClassNames({
            'navbar-collapse': true,
            collapse: !this.state.navBarOpen
        });

        var currentUser = this.state.user.username;

        console.log(UserStore.getState())

        return (
            <div className="navbar navbar-default navbar-fixed-top">
                <div className="container">
                    <div className="navbar-header">
                        <Link className="navbar-brand" to="home">
                            <img className="navbar-logo" src="/public/media/logo-square.png" />
                            <span className="navbar-brand-label">Ignitor</span>
                        </Link>
                        <button
                            className="navbar-toggle collapsed"
                            onClick={this.toggleMenu}>

                            <span className="icon-bar"></span>
                            <span className="icon-bar"></span>
                            <span className="icon-bar"></span>
                        </button>
                    </div>
                    <div className={navBarCollapse}>
                        <ul className="nav navbar-nav">
                            <li className={this.isNavActive(['home'])}>
                                <Link to="home">My account</Link>
                            </li>
                            <li className={this.isNavActive(['settings'])}>
                                <Link to="settings">Settings</Link>
                            </li>
                            <li className={this.isNavActive(['posts', 'postDetails'])}>
                                <Link to="posts">Posts</Link>
                            </li>
                        </ul>
                        <ul className="nav navbar-nav navbar-right">
                            <li>
                                <a href="/login/logout">Sign out</a>
                            </li>
                            <li>
                               <a href="{javascript:void(0)}">Hello {currentUser}</a>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        );
    }
});
```


module.exports = Component;
