<template>
	<main id="sys_app_refresh_form">
		<mm_grid>
			<mm_col width="33">
				<mm_form class="card">
					<div class="head arrow">
						<h5>{{ form[field] ? '修改' : '创建' }}应用刷新</h5>
					</div>
					<div class="body">
						<dl>
							<dt>用户</dt>
							<dd>
								<mm_select v-model="form.user_id" :options="$to_kv(list_account, 'user_id', 'nickname')" />
							</dd>
							<dt class="required">应用ID</dt>
							<dd>
								<mm_input v-model="form.appid" :minlength="0" :maxlength="0" placeholder=""
								 :required="true" />
							</dd>
							<dt class="required">刷新令牌</dt>
							<dd>
								<mm_input v-model="form.refresh_token" :minlength="0" :maxlength="0" placeholder="用来刷新访问牌，保留30天"
								 :required="true" />
							</dd>
						</dl>
					</div>
					<div class="foot">
						<div class="mm_group">
							<button class="btn_default" type="button" @click="cancel">取消</button>
							<button class="btn_primary" type="button" @click="submit()">提交</button>
						</div>
					</div>
				</mm_form>
			</mm_col>
		</mm_grid>
	</main>
</template>


<script>
	import mixin from '/src/mixins/page.js';

	export default {
		mixins: [mixin],
		components: {},
		data() {
			return {
				url_submit: "/apis/sys/app_refresh?",
				url_get_obj: "/apis/sys/app_refresh?method=get_obj",
				field: "refresh_id",
				query: {
					"refresh_id": 0
				},
				form: {
					"refresh_id": 0,
					"user_id": 0,
					"appid": '',
					"refresh_token": '',
				},
				// 用户
				'list_account': [ ],
			}
		},
		methods: {
			/**
			 * 获取用户
			 * @param {query} 查询条件
			 */
			get_account(query) {
				var _this = this;
				if (!query) {
					query = {
						field: "user_id,nickname"
					};
				}
				this.$get('~/apis/user/account?size=0', query, function(json) {
					if (json.result) {
						_this.list_account .clear();
						_this.list_account .addList(json.result.list)
					}
				});
			},
		},
		created() {
			// 获取用户
			this.get_account();
		}
	}
</script>

<style>
</style>
