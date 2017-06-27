# rxjs-forkJoin
angular2/4 开发中如何一起拿接口数据并渲染
等价于 $#q.all  (Observable.forkJoin)

一、注入所需 设计模式之观察者模式(Observable)
import { Observable } from 'rxjs/Observable';

二、(Observable.forkJoin)此展示为开发模式之数据流展示效果，用法：
        let FollowAppStream = this.subappService.FollowApp(appkey);

        let UpdateDefaultAppStream = this.subappService.UpdateDefaultApp(appkey, true);

        let DataStream = Observable.forkJoin(FollowAppStream, UpdateDefaultAppStream);

        let onOK = r => {
            this.followapp = r[0];
            this.default = r[1];
            this.GetData();
        };

        return [DataStream.subscribe(onOK)];
 
### 一个接一个拿接口数据参考代码(takeUntil)

        let loginStream = this.userService.login(account, this.loginModel.userPwd);

        let subAppListStream = this.subappService.SubAppList(true);

        let onOK = r => this.goRedirectUrl(r.SubAppList);

        let onErr = err => Alert.error('', err["ErrorMessage"]);

        return [
            subAppListStream.takeUntil(loginStream).subscribe(onOK, onErr)
        ];
