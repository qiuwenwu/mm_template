<template>
	<main id="user_info">
		<mm_grid>
			<mm_col>
				<mm_view>
					<header class="arrow">
						<h5>用户信息</h5>
					</header>
					<mm_body>
						<mm_form class="mm_filter">
							<h5><span>筛选条件</span></h5>
							<mm_list col="3">
								<mm_col>
									<mm_input v-model="query.keyword" title="关键词" desc="姓名"
									 @blur="search()" />
								</mm_col>
								<mm_col>
									<mm_select v-model="query.sex" title="性别" :options="$to_kv(arr_sex)" @change="search()" />
								</mm_col>
								<mm_col>
									<mm_select v-model="query.idcard_state" title="身份实名认证" :options="$to_kv(arr_idcard_state)" @change="search()" />
								</mm_col>
								<mm_col>
									<mm_select v-model="query.province_id" title="省份" :options="$to_kv(list_address_province, 'province_id', 'name')"
									 @change="search()" />
								</mm_col>
								<mm_col>
									<mm_select v-model="query.city_id" title="所在城市" :options="$to_kv(list_address_city, 'city_id', 'name')"
									 @change="search()" />
								</mm_col>
								<mm_col>
									<mm_btn class="btn_primary-x" type="reset" @click.native="reset();search()">重置</mm_btn>
								</mm_col>
							</mm_list>
						</mm_form>
						<div class="mm_action">
							<h5><span>操作</span></h5>
							<div class="">
								<mm_btn class="btn_primary-x" url="./info_form">添加</mm_btn>
								<mm_btn @click.native="show = true" class="btn_primary-x" v-bind:class="{ 'disabled': !selects }">批量修改</mm_btn>
							</div>
						</div>
						<mm_table type="2">
							<thead>
								<tr>
									<th scope="col" class="th_selected"><input type="checkbox" :checked="select_state" @click="select_all()" /></th>
									<th scope="col" class="th_id"><span>#</span></th>
									<th scope="col">
										<mm_reverse title="性别" v-model="query.orderby" field="sex" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="身份实名认证" v-model="query.orderby" field="idcard_state" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="年龄" v-model="query.orderby" field="age" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="省份" v-model="query.orderby" field="province_id" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="所在城市" v-model="query.orderby" field="city_id" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="生日" v-model="query.orderby" field="birthday" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="姓名" v-model="query.orderby" field="name" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="职业" v-model="query.orderby" field="job" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="毕业学校" v-model="query.orderby" field="school" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="所学专业" v-model="query.orderby" field="major" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="身份证号" v-model="query.orderby" field="idcard" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="公司地址" v-model="query.orderby" field="company_address" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="详细地址" v-model="query.orderby" field="address" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="工作范围" v-model="query.orderby" field="job_scope" :func="search"></mm_reverse>
									</th>
									<th scope="col">
										<mm_reverse title="公司经营范围" v-model="query.orderby" field="company_business" :func="search"></mm_reverse>
									</th>
									<th scope="col" class="th_handle"><span>操作</span></th>
								</tr>
							</thead>
							<draggable v-model="list" tag="tbody" @change="sort_change">
								<tr v-for="(o, idx) in list" :key="idx" :class="{'active': select == idx}" @click="selected(idx)">
									<th scope="row"><input type="checkbox" :checked="select_has(o[field])" @click="select_change(o[field])" /></th>
									<td>
										<span>{{ o.user_id }}</span>
									</td>
									<td>
										<span>{{arr_sex[o.sex] }}</span>
									</td>
									<td>
										<span>{{arr_idcard_state[o.idcard_state] }}</span>
									</td>
									<td>
										<span>{{ o.age }}</span>
									</td>
									<td>
										<span>{{ get_name(list_address_province, o.province_id, 'province_id', 'name') }}</span>
									</td>
									<td>
										<span>{{ get_name(list_address_city, o.city_id, 'city_id', 'name') }}</span>
									</td>
									<td>
										<span>{{ $to_time(o.birthday, 'yyyy-MM-dd') }}</span>
									</td>
									<td>
										<span>{{ o.name }}</span>
									</td>
									<td>
										<span>{{ o.job }}</span>
									</td>
									<td>
										<span>{{ o.school }}</span>
									</td>
									<td>
										<span>{{ o.major }}</span>
									</td>
									<td>
										<span>{{ o.idcard }}</span>
									</td>
									<td>
										<span>{{ o.company_address }}</span>
									</td>
									<td>
										<span>{{ o.address }}</span>
									</td>
									<td>
										<span>{{ o.job_scope }}</span>
									</td>
									<td>
										<span>{{ o.company_business }}</span>
									</td>
									<td>
										<mm_btn class="btn_primary" :url="'./info_form?user_id=' + o[field]">修改</mm_btn>
										<mm_btn class="btn_warning" @click.native="del_show(o, field)">删除</mm_btn>
									</td>
								</tr>
							</draggable>
						</mm_table>
					</mm_body>
					<footer>
						<mm_grid class="mm_data_count">
							<mm_col>
								<mm_select v-model="query.size" :options="$to_size()" @change="search()" />
							</mm_col>
							<mm_col width="50" style="min-width: 22.5rem;">
								<mm_pager display="2" v-model="query.page" :count="count / query.size" :func="goTo" :icons="['首页', '上一页', '下一页', '尾页']"></mm_pager>
							</mm_col>
							<mm_col>
								<div class="right plr">
									<span class="mr">共 {{ count }} 条</span>
									<span>当前</span>
									<input class="pager_now" v-model.number="page_now" @blur="goTo(page_now)" @change="page_change" />
									<span>/{{ page_count }}页</span>
								</div>
							</mm_col>
						</mm_grid>
					</footer>
				</mm_view>
			</mm_col>
		</mm_grid>
		<mm_modal v-model="show" mask="true">
			<mm_view class="card bg_no">
				<header class="bg_white">
					<h5>批量修改</h5>
				</header>
				<mm_body>
					<dl>
						<dt>性别</dt>
						<dd>
							<mm_select v-model="form.sex" :options="$to_kv(arr_sex)" />
						</dd>
						<dt>身份实名认证</dt>
						<dd>
							<mm_select v-model="form.idcard_state" :options="$to_kv(arr_idcard_state)" />
						</dd>
						<dt>省份</dt>
						<dd>
							<mm_select v-model="form.province_id" :options="$to_kv(list_address_province, 'province_id', 'name')" />
						</dd>
						<dt>所在城市</dt>
						<dd>
							<mm_select v-model="form.city_id" :options="$to_kv(list_address_city, 'city_id', 'name')" />
						</dd>
					</dl>
				</mm_body>
				<footer>
					<div class="mm_group">
						<button class="btn_default" type="reset" @click="show = false">取消</button>
						<button class="btn_primary" type="button" @click="batchSet()">提交</button>
					</div>
				</footer>
			</mm_view>
		</mm_modal>
	</main>
</template>

<script>
	import mixin from '/src/mixins/page.js';

	export default {
		mixins: [mixin],
		data() {
			return {
				// 列表请求地址
				url_get_list: "/apis/user/info",
				url_del: "/apis/user/info?method=del&",
				url_set: "/apis/user/info?method=set&",
				field: "user_id",
				query_set: {
					"user_id": ""
				},
				// 查询条件
				query: {
					//页码
					page: 1,
					//页面大小
					size: 10,
					// 用户ID
					'user_id': 0,
					// 性别——最小值
					'sex_min': '',
					// 性别——最大值
					'sex_max': '',
					// 身份实名认证——最小值
					'idcard_state_min': '',
					// 身份实名认证——最大值
					'idcard_state_max': '',
					// 年龄——最小值
					'age_min': 0,
					// 年龄——最大值
					'age_max': 0,
					// 生日——开始时间
					'birthday_min': '',
					// 生日——结束时间
					'birthday_max': '',
					// 姓名
					'name': '',
					// 关键词
					'keyword': '',
					//排序
					orderby: ""
				},
				form: {},
				//颜色
				arr_color: ['', '', 'font_yellow', 'font_success', 'font_warning', 'font_primary', 'font_info', 'font_default'],
				// 性别
				'arr_sex': [ '未设置' , '男' , '女' ],
				// 身份实名认证
				'arr_idcard_state': [ '' , '未认证' , '认证中' , '认证通过' ],
				// 省份
				'list_address_province': [ ],
				// 所在城市
				'list_address_city': [ ],
				// 视图模型
				vm: {}
			}
		},
		methods: {
			/**
			 * 获取省份
			 * @param {query} 查询条件
			 */
			get_address_province(query) {
				var _this = this;
				if (!query) {
					query = {
						field: "province_id,name"
					};
				}
				this.$get('~/apis/sys/address_province?size=0', query, function(json) {
					if (json.result) {
						_this.list_address_province .clear();
						_this.list_address_province .addList(json.result.list)
					}
				});
			},
			/**
			 * 获取所在城市
			 * @param {query} 查询条件
			 */
			get_address_city(query) {
				var _this = this;
				if (!query) {
					query = {
						field: "city_id,name"
					};
				}
				this.$get('~/apis/sys/address_city?size=0', query, function(json) {
					if (json.result) {
						_this.list_address_city .clear();
						_this.list_address_city .addList(json.result.list)
					}
				});
			},
		},
		created() {
			// 获取省份
			this.get_address_province();
			// 获取所在城市
			this.get_address_city();
		}
	}
</script>

<style>
</style>
