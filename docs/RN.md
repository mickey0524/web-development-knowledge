* xcode解决无法真机调试的问题
    
    [xcode解决真机无法调试的问题](https://stackoverflow.com/questions/39524148/requires-a-development-team-select-a-development-team-in-the-project-editor-cod)，主要是要设置personal dev account，然后要修改RN给你的项目起的默认package的名字

* React Navigation

    [Navigation官方文档](https://reactnavigation.org/docs/navigators/stack)

* RN中强制竖屏显示方法

    * IOS把info.plist的UISupportedInterfaceOrientations中UIInterfaceOrientationLandscapeLeft、UIInterfaceOrientationLandscapeRight注释掉
    * Android在AndroidManifest中的MainActivity后面增加android:screenOrientation="portrait"

* RN中TabNavigation 文字上下文不居中的问题，需要给tabStyle设置height值

    ```js
    tabStyle: {
        height: 30,
    },
    style: {
        height: 40,
        backgroundColor: '#FFF',
        borderBottomWidth: 1 / dpr,
        borderColor: '#D8D8D8',
        borderStyle: 'solid',
    },
    ```
* RN中FLatList设置getItemLayout导致仅仅渲染初始数目的item，后面渲染出空白

    [VirtualizedList + getItemLayout only renders initialNumToRender items](https://github.com/facebook/react-native/issues/15734)

* RN中drawerNavigation初始化报routeName undefined，^1.0.0-beta.22版本，需要手动添加drawerOpenRoute，drawerCloseRoute，drawerToggleRoute贼坑

    [https://github.com/react-navigation/react-navigation/issues/3095]

* RN中react-redux 的 mapDispatchToProps 写法

    ```js
    import { connect } from 'react-redux'; 

    const mapDispatchToProps = (dispatch) => {
        action: (param) => { dispatch(action(param)) }
    }
    ```

* 利用Android的adb工具免数据线调试

    在Android -> SDK -> platform-tools文件夹下运行 `adb reverse tcp:8081 tcp:8081`